<a id="azure-virtual-networking-nsgs-and-private-dns-az-104-lab"></a>

# Azure Virtual Networking, NSGs and Private DNS (AZ-104 Lab)

> A hands-on Azure lab where I explored network security groups, application security groups and private DNS in a small UK South network, and tracked down why IP flow verify kept denying traffic that my allow rule should have matched.

\---

<a id="table-of-contents"></a>

## Table of Contents

* [Why I Built This Lab](#why-i-built-this-lab)
* [What I Wanted to Learn](#what-i-wanted-to-learn)
* [Lab Environment](#lab-environment)
* [Architecture](#architecture)
* [Implementation](#implementation)

  * [Step 1 - Check the VM is running](#step-1)
  * [Step 2 - Add the VM's network interface to the ASG](#step-2)
  * [Step 3 - Review the private DNS zone](#step-3)
  * [Step 4 - Add the app01 record and link the VNet](#step-4)
  * [Step 5 - Run IP flow verify for the first time](#step-5)
  * [Step 6 - Check the ASG and effective security rules](#step-6)
  * [Step 7 - Run NSG diagnostics](#step-7)
  * [Step 8 - Read the rule back](#step-8)
  * [Step 9 - Fix the destination port and retest](#step-9)
  * [Step 10 - Test private DNS from the VM](#step-10)
  * [Step 11 - Query a DNS server from Cloud Shell](#step-11)
* [Challenges During the Lab](#challenges-during-the-lab)
* [Key Takeaways](#key-takeaways)
* [Skills Practiced](#skills-practiced)

\---

<a id="why-i-built-this-lab"></a>

## Why I Built This Lab

I work on a service desk and I am studying for AZ-104 so I can move into a cloud engineer role. Reading about NSGs only gets me so far. I wanted to build a small network, put a rule in front of a VM and then test it.

The test did not pass at first. That turned the lab into a troubleshooting exercise, and it taught me more than a clean run would have.

The goal was simple:

> Control inbound traffic to a Linux VM with an NSG, prove it with Network Watcher, and check that private DNS works inside the virtual network.

<a id="what-i-wanted-to-learn"></a>

## What I Wanted to Learn

* How NSG rules are evaluated and how priority works
* What an application security group does compared with an NSG
* How to use IP flow verify, effective security rules and NSG diagnostics
* How to find one wrong setting when everything seems right
* How a private DNS zone linked to a virtual network resolves a name from a VM

<a id="lab-environment"></a>

## Lab Environment

|Component|Configuration|
|-|-|
|Subscription|`AZ104-subscription`|
|Resource group|`rg-az104-lab-uksouth`|
|Region|`UK South`|
|Virtual network|`vnet-az104-lab`|
|Subnet|`default`|
|Virtual machine|`vm-az104-lin01`|
|VM size|`Standard F1als v7` (1 vCPU, 2 GiB memory)|
|Operating system|`Linux` (generation V2, x64)|
|Network interface|`vm-az104-lin01202`|
|Private IP address|`10.10.1.4`|
|Public IP address|`None`|
|Network security group|`nsg-az104-lab` (applied through the `default` subnet)|
|Application security group|`asg-mgmt-vms`|
|Custom NSG rule|`Allow-HTTP-From-MyIP`, priority `1010`, TCP, Allow|
|Rule destination port|`8080` at first, `80` after the fix|
|Private DNS zone|`az104lab.internal`|
|Private DNS record|`app01`, A record, `10.10.1.4`|
|Virtual network link|`link-vnet-az104-lab`|

<a id="architecture"></a>

## Architecture

```text
Subscription: AZ104-subscription
└── Resource group: rg-az104-lab-uksouth (UK South)
    ├── vnet-az104-lab
    │   └── subnet: default  <-- nsg-az104-lab
    │       └── vm-az104-lin01 (NIC vm-az104-lin01202, 10.10.1.4)
    │           └── member of asg-mgmt-vms
    ├── nsg-az104-lab
    │   └── Allow-HTTP-From-MyIP (priority 1010)
    ├── asg-mgmt-vms
    └── Private DNS zone: az104lab.internal
        ├── app01 (A) -> 10.10.1.4
        └── link-vnet-az104-lab -> vnet-az104-lab
```

The NSG applies to the VM through the subnet. The private DNS zone is linked to the virtual network so the VM can resolve `app01`.

<a id="implementation"></a>

## Implementation

<a id="step-1"></a>

### Step 1 - Check the VM is running

I opened the overview page for `vm-az104-lin01` to check it had deployed and was running.

**Screenshot**

!\[VM overview showing the VM running](./vm-running-overview.png)

**Observation**

The VM was Running in UK South, size `Standard F1als v7`, in `vnet-az104-lab/default`, with private IP `10.10.1.4` and no public IP. 

\---

<a id="step-2"></a>

### Step 2 - Add the VM's network interface to the ASG

I checked the application security group `asg-mgmt-vms` before and after adding the VM's network interface to it.

**Screenshot**

!\[ASG with no network interfaces](./asg-empty.png)

!\[ASG with the VM network interface attached](./asg-with-vm-nic.png)

**Why This Matters**

An application security group lets me group network interfaces and use the group in a rule instead of IP addresses. My rule uses Destination Any, so the ASG does not affect it in this lab.

\---

<a id="step-3"></a>

### Step 3 - Review the private DNS zone

I opened the private DNS zone `az104lab.internal` in the same resource group.

**Screenshot**

!\[Private DNS zone overview](./private-dns-zone.png)

**Observation**

The zone shows 2 record sets and 1 virtual network link. It shows 0 links with registration, which fits the link having auto-registration disabled.

\---

<a id="step-4"></a>

### Step 4 - Add the app01 record and link the VNet

The private zone has an A record called `app01` that points to `10.10.1.4`. The zone is also linked to `vnet-az104-lab`.

**Screenshot**

!\[Private DNS record sets showing app01](./private-dns-app01-record.png)

!\[Private DNS virtual network link](./private-dns-vnet-link.png)

**Key Detail**

The `app01` record has a TTL of 3600 and Auto registered is False. The link `link-vnet-az104-lab` shows Completed, with auto-registration and fallback to internet both Disabled.

\---

<a id="step-5"></a>

### Step 5 - Run IP flow verify for the first time

In Network Watcher I ran IP flow verify for inbound TCP traffic to `10.10.1.4` on local port 80.

**Screenshot**

!\[IP flow verify returning Access denied](./ip-flow-verify-denied.png)

**Result**

The result was Access denied, matched by `DenyAllInBound`. The remote IP in this screenshot does not match the source address in my rule, so this run on its own does not test the rule.

\---

<a id="step-6"></a>

### Step 6 - Check the ASG and effective security rules

I checked that the VM's network interface belongs to `asg-mgmt-vms`. Then I opened Effective security rules for that interface.

**Screenshot**

!\[VM application security group page](./vm-asg-association.png)

!\[Effective security rules for the network interface](./effective-rules-wrong-port.png)

**Key Detail**

`nsg-az104-lab` applies through the subnet, and `Allow-HTTP-From-MyIP` at priority 1010 sits above `DenyAllInBound` at 65500. The same row shows source ports 80-80 and destination ports 8080-8080, which I did not notice until later.

\---

<a id="step-7"></a>

### Step 7 - Run NSG diagnostics

I ran NSG diagnostics in Network Watcher for `vm-az104-lin01`, using inbound TCP from my public IP to `10.10.1.4` on port 80.

**Screenshot**

!\[NSG diagnostics showing the allow rule not matched](./nsg-diagnostics-denied.png)

**Why This Matters**

The tool explains that a rule only applies when it matches on protocol, source, source port, destination and destination port. It marked `Allow-HTTP-From-MyIP` as Not applied, with source port and destination port both Not matched, so `DenyAllInBound` in `nsg-az104-lab` decided the result.

\---

<a id="step-8"></a>

### Step 8 - Read the rule back

I opened `Allow-HTTP-From-MyIP` in `nsg-az104-lab` and read each field back one at a time.

**Screenshot**

!\[Rule showing destination port 8080](./nsg-rule-wrong-port-8080.png)

**Result**

The destination port range was 8080, not 80, so traffic to port 80 could never match. The source port range was also set to 80.

\---

<a id="step-9"></a>

### Step 9 - Fix the destination port and retest

The saved rule now shows Service as HTTP with destination port 80. I then reran IP flow verify for inbound traffic to `10.10.1.4` on local port 80.

**Screenshot**

!\[Rule with Service set to HTTP and destination port 80](./nsg-rule-fixed-port-80.png)

!\[IP flow verify returning Access allowed](./ip-flow-verify-allowed.png)

**Result**

IP flow verify returned Access allowed, matched by `Allow-HTTP-From-MyIP` in `nsg-az104-lab`. The rule's source port range still shows 80, and I used remote port 80 in the test, so the two match. Real clients usually connect from a random source port, so a real web rule would normally use Any (`\\\*`) there, and I have not tested that.

\---

<a id="step-10"></a>

### Step 10 - Test private DNS from the VM

I used Run command on `vm-az104-lin01` to run a shell script inside the VM. The script looked up the private DNS name.

```bash
nslookup app01.az104lab.internal
```

**Screenshot**

!\[Run command output resolving app01](./nslookup-private-dns.png)

**Result**

The VM resolved `app01.az104lab.internal` to `10.10.1.4`, which matches the A record from Step 4. The server shown is `127.0.0.53`, the VM's local resolver, and the answer is marked non-authoritative.

\---

<a id="step-11"></a>

### Step 11 - Query a DNS server from Cloud Shell

From Azure Cloud Shell I looked up `www.az104-lab.example` and told `nslookup` to ask the server at `13.107.236.6` instead of the default resolver. This ran in Cloud Shell, not on the VM.

```bash
nslookup www.az104-lab.example 13.107.236.6
```

**Screenshot**

!\[Cloud Shell nslookup against a specific server](./nslookup-public-dns.png)

**Observation**

The server answered with `203.0.113.10`, and the output has no non-authoritative line. The screenshots do not show which DNS zone holds this record.

\---

<a id="challenges-during-the-lab"></a>

## Challenges During the Lab

* IP flow verify returned `DenyAllInBound` even though `Allow-HTTP-From-MyIP` existed at priority 1010 and appeared in the effective security rules.
* Checking the subnet association, the NIC, the source address, the destination and the priority found nothing wrong.
* The effective security rules row already showed destination ports 8080-8080, but I read past it.
* NSG diagnostics marked the ports as Not matched. Reading the rule back showed a destination port range of 8080 instead of 80.
* The same rule has a source port range of 80, which is unusual for web traffic.
* The VM ended up in the `default` subnet instead of `snet-management`, which the original lab instructions specified. I left it as it is.

The main lesson was that reading each setting back is faster than assuming it is right.

<a id="key-takeaways"></a>

## Key Takeaways

This lab showed me how small a networking fault can be and how easy it is to miss when I trust what I think I set.

The biggest lessons I took away were:

* An NSG rule must match on every criterion: protocol, source, source port, destination and destination port. One wrong value lets `DenyAllInBound` win.
* Effective security rules show what is applied to the NIC, but I still have to read every column.
* NSG diagnostics shows which criteria failed. IP flow verify only names the rule that decided.
* An application security group only matters if a rule uses it. Mine used Destination Any.
* Clients usually connect from a random source port, so a web rule normally needs a source port range of Any.
* Once the private DNS zone was linked to the VNet, the VM resolved `app01` to `10.10.1.4`.
* `nslookup` can be pointed at a specific DNS server by adding its address to the command.

<a id="skills-practiced"></a>

## Skills Practiced

<a id="networking"></a>

### Networking

* Editing NSG inbound rules
* Understanding rule priority and the default rules
* Working with an NSG applied through a subnet
* Adding a NIC to an application security group
* Reading effective security rules for a network interface

<a id="troubleshooting"></a>

### Troubleshooting

* Using Network Watcher IP flow verify
* Using Network Watcher NSG diagnostics
* Checking a rule field by field
* Rerunning a test after one change

<a id="dns"></a>

### DNS

* Working with a private DNS zone, an A record and a virtual network link
* Testing name resolution with `nslookup` from a VM and from Cloud Shell

<a id="virtual-machines"></a>

### Virtual Machines

* Checking VM status, size and network details
* Running a script with Run command

