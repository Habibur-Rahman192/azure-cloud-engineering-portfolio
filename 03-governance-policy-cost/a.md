Azure Governance and Resource Management (AZ-104 Lab 03)
> A hands-on Azure lab where I explored how administrators enforce standards and protect resources in practice, using Azure Policy, Resource Locks, Management Groups, RBAC and Azure Resource Manager.
---
Table of Contents
Why I Built This Lab
What I Wanted to Learn
Lab Environment
Architecture
Implementation
Step 1 - Restrict Deployments with Allowed Locations
Step 2 - Test the Location Restriction
Step 3 - Review the Built-in Tag Policy
Step 4 - Create a Custom Audit Policy
Step 5 - Review Policy Compliance
Step 6 - Configure Automatic Tag Remediation
Step 7 - Verify Remediation
Step 8 - Protect Resources with Locks
Step 9 - Test the Resource Lock
Step 10 - Organise the Subscription with a Management Group
Step 11 - Review RBAC Inheritance
Step 12 - Move a Resource Between Resource Groups
Challenges During the Lab
Key Takeaways
Skills Practiced
---
Why I Built This Lab
While preparing for the AZ-104 certification, I wanted to move beyond deploying resources and see how governance works in practice.
I understood the theory behind Azure Policy, locks and management groups, but I wanted to build a small environment and test each control myself.
The goal was simple:
> Enforce a rule, prove it blocks or reports what it should, fix what is non-compliant, and protect the resources that matter.
This lab helped me understand how Azure administrators keep an environment consistent and safe as it grows.
---
What I Wanted to Learn
How Azure Policy blocks non-compliant deployments
The difference between Deny, Audit and Modify effects
How policy compliance is reported
How remediation works with a managed identity
How resource locks protect resources
How management groups organise subscriptions
How RBAC permissions inherit across scopes
How resources are moved between resource groups
---
Lab Environment
Component	Configuration
Azure Services	Azure Policy, Resource Locks, Management Groups, RBAC
Subscription	`AZ104-subscription`
Resource Group	`rg-az104-lab-uksouth`
Region	UK South
Management Group	`mg-az104-lab`
Test Resources	`vnet-az104-lab`, `vnet-mgmt-az104-lab`, `nsg-az104-lab`, `nsg-move-test`
Test Method	Azure Portal and Azure PowerShell
---
Architecture
The environment was kept simple so I could focus on how each governance control behaves.
```text
Tenant Root Group
      │
      ▼
mg-az104-lab (Management Group)
      │
      ▼
AZ104-subscription
      │
      ▼
rg-az104-lab-uksouth (Resource Group)
      │
      ├── Azure Policy: Allowed locations, tag audit, tag inheritance
      ├── Resource Lock: CanNotDelete
      ▼
Virtual Networks and Network Security Groups
```
Policies and locks were applied at resource group scope, and the subscription was moved under a custom management group so governance can be applied from a higher level.
---
Implementation
Step 1 - Restrict Deployments with Allowed Locations
I assigned the built-in Allowed locations policy to `rg-az104-lab-uksouth`, allowing only `uksouth` and using the `Deny` effect.
Screenshot
![Allowed Locations Assignment](./Azure%20Policy%20Assignment%20-%20Allowed%20Locations.png)
![Allowed Locations Parameters](./Allowed%20Locations%20Policy%20Configuration.png)
What I Learned
Azure Policy can enforce rules before a resource is created, not just report on it afterwards.
---
Step 2 - Test the Location Restriction
To confirm the policy worked, I tried to create a storage account in UK West.
Screenshot
![Storage Account Blocked](./Azure%20Policy%20Enforcement%20-%20Storage%20Account%20Deployment%20Blocked.png)
Result
The portal flagged the region as blocked by the Allowed locations policy, so the deployment could not go ahead.
---
Step 3 - Review the Built-in Tag Policy
Next, I looked at the built-in Require a tag and its value on resources policy.
Screenshot
![Built-in Tag Policy](./Azure%20Policy%20Definition%20-%20Require%20a%20Tag%20and%20Its%20Value%20on%20Resources.png)
What I Learned
This built-in policy only supports the `Deny` effect. If I only wanted to report missing tags without blocking deployments, I needed a custom policy.
---
Step 4 - Create a Custom Audit Policy
I created a custom policy called Require a tag on resources using the `Audit` effect.
Screenshot
![Custom Audit Policy](./Custom%20Azure%20Policy%20Definition%20for%20Resource%20Tag%20Auditing.png)
Why This Matters
Audit policies let administrators see where a standard is not being met without disrupting anyone's work.
---
Step 5 - Review Policy Compliance
I then checked how Azure reported compliance across the subscription.
Screenshot
![Compliance Dashboard](./Azure%20Policy%20Compliance%20Dashboard%20Showing%20Policy%20Enforcement%20Results.png)
![Non-compliant Resource Details](./Policy%20Compliance%20Details%20for%20Resources%20Missing%20Required%20Environment%20Tags.png)
Observation
The tag policy showed 67% compliance (2 of 3 resources), while Allowed locations showed 100%.
The detail view showed `vnet-mgmt-az104-lab` was missing the `Environment` tag, with the reason clearly listed.
---
Step 6 - Configure Automatic Tag Remediation
To fix the missing tags automatically, I assigned the Inherit a tag from the resource group if missing policy, which uses the `Modify` effect.
Screenshot
![Managed Identity Assignment](./Azure%20Policy%20Assignment%20with%20Managed%20Identity%20for%20Automatic%20Tag%20Inheritance.png)
Key Detail
`Modify` policies need a managed identity to change existing resources. I used a system-assigned identity, which was granted Contributor on `rg-az104-lab-uksouth`.
---
Step 7 - Verify Remediation
After remediation ran, I checked the compliance view again.
Screenshot
![Compliance After Remediation](./Azure%20Policy%20Compliance%20After%20Automatic%20Tag%20Inheritance%20Remediation.png)
Result
The inherit-tag policy reported 100% compliance (3 of 3 resources), confirming the tags had been applied automatically.
---
Step 8 - Protect Resources with Locks
I applied a `CanNotDelete` lock to `vnet-az104-lab` in the portal, then created a second lock with Azure PowerShell.
Screenshot
![Lock on Virtual Network](./CanNotDelete%20Resource%20Lock%20Applied%20to%20Azure%20Virtual%20Network.png)
```powershell
New-AzResourceLock `
    -LockName "lock-vnet-az104-lab" `
    -LockLevel CanNotDelete `
    -ResourceGroupName "rg-az104-lab-uksouth"
```
![PowerShell Lock Creation](./Creating%20a%20CanNotDelete%20Resource%20Lock%20Using%20Azure%20PowerShell.png)
What I Learned
Because the PowerShell command uses `-ResourceGroupName` and no resource name, it locks the whole resource group rather than just the virtual network. The lock name should reflect its scope.
---
Step 9 - Test the Resource Lock
I tried to delete `vnet-az104-lab` while the lock was in place.
Screenshot
![Deletion Blocked](./Resource%20Deletion%20Blocked%20by%20CanNotDelete%20Resource%20Lock.png)
Result
Azure refused the deletion and asked for the lock to be removed first.
---
Step 10 - Organise the Subscription with a Management Group
I created a management group called `mg-az104-lab` and moved the subscription into it.
Screenshot
Before, with the subscription directly under the Tenant Root Group:
![Management Groups Before](./Azure%20Management%20Group%20Hierarchy%20and%20Subscription%20Organization.png)
After, with the subscription under `mg-az104-lab`:
![Subscription Under Management Group](./Azure%20Subscription%20Assigned%20to%20the%20mg-az104-lab%20Management%20Group.png)
What I Learned
Management groups sit above subscriptions, so governance can be applied once and inherited by everything beneath.
---
Step 11 - Review RBAC Inheritance
I reviewed the Owner role assignments to see how access flows down from the management group to the subscription.
Screenshot
![RBAC Role Assignments](./Screenshot%202026-11-22%20100503.png)
Observation
Permissions assigned at a higher scope are inherited by the scopes below it, which keeps access management consistent.
---
Step 12 - Move a Resource Between Resource Groups
Finally, I moved `nsg-move-test` from `rg-az104-move-test` to `rg-az104-lab-uksouth`.
Screenshot
![Move Review](./Azure%20Resource%20Move%20Review%20Showing%20Source%20and%20Target%20Resource%20Groups.png)
![NSG After Move](./Network%20Security%20Group%20Successfully%20Moved%20to%20the%20Target%20Resource%20Group.png)
Result
The NSG now appears in the target resource group within the same subscription.
---
⚠ Challenges During the Lab
A few things initially caught me out:
Assigning a policy did not fix existing resources. A remediation task was needed.
`Modify` policies would not work without a managed identity with enough permissions.
It took a while to separate what `Deny`, `Audit` and `Modify` each do.
My PowerShell lock turned out to be at resource group scope, not on the virtual network.
A lock blocks deletion but does not stop anyone reading the resource.
Although these were small issues, they helped me understand how Azure governance behaves in real environments.
---
Key Takeaways
This lab gave me a much better understanding of Azure governance than simply reading documentation.
The biggest lessons I took away were:
Use Deny to prevent, Audit to report and Modify to fix.
Always test that a policy or lock actually does what you expect.
Managed identities let remediation run without stored credentials.
Locks add protection on top of RBAC permissions.
Management groups and inherited RBAC make governance easier to scale.
Check the scope of every assignment and lock before relying on it.
---
Skills Practiced
Azure Governance
Azure Policy
Policy Compliance Monitoring
Policy Remediation
Resource Tag Governance
Security and Access Control
Azure RBAC
RBAC Inheritance
Resource Locks
Management Groups
Azure Administration
Azure Resource Manager
Resource Group Management
Moving Resources
Azure PowerShell
---