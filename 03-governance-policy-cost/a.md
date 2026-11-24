\# AZ-104 Lab 03: Azure Governance, Policy and Resource Management



\## Overview



In this lab, I focused on Azure governance and resource management by implementing Azure Policy, Resource Locks, Management Groups, RBAC inheritance, and resource relocation.



Rather than simply deploying resources, I wanted to understand how Azure administrators enforce standards, protect critical infrastructure, and maintain compliance across an Azure environment. This lab provided practical experience with governance capabilities that are commonly used in enterprise Azure environments.



\---



\## Table of Contents



\- \[Why I Built This Lab](#why-i-built-this-lab)

\- \[Business Scenario](#business-scenario)

\- \[What I Wanted to Learn](#what-i-wanted-to-learn)

\- \[Lab Environment](#lab-environment)

\- \[Implementation](#implementation)

&#x20; - \[Step 1 - Configure the Allowed Locations Policy](#step-1---configure-the-allowed-locations-policy)

&#x20; - \[Step 2 - Validate Policy Enforcement](#step-2---validate-policy-enforcement)

&#x20; - \[Step 3 - Configure a Required Resource Tag Policy](#step-3---configure-a-required-resource-tag-policy)

&#x20; - \[Step 4 - Create a Custom Audit Policy](#step-4---create-a-custom-audit-policy)

&#x20; - \[Step 5 - Review Policy Compliance](#step-5---review-policy-compliance)

&#x20; - \[Step 6 - Configure Automatic Tag Remediation](#step-6---configure-automatic-tag-remediation)

&#x20; - \[Step 7 - Validate Policy Remediation](#step-7---validate-policy-remediation)

&#x20; - \[Step 8 - Protect Resources Using Resource Locks](#step-8---protect-resources-using-resource-locks)

&#x20; - \[Step 9 - Validate Resource Lock Enforcement](#step-9---validate-resource-lock-enforcement)

&#x20; - \[Step 10 - Create and Configure a Management Group](#step-10---create-and-configure-a-management-group)

&#x20; - \[Step 11 - Review RBAC Inheritance](#step-11---review-rbac-inheritance)

&#x20; - \[Step 12 - Move Resources Between Resource Groups](#step-12---move-resources-between-resource-groups)

\- \[Validation and Testing](#validation-and-testing)

\- \[Challenges During the Lab](#challenges-during-the-lab)

\- \[Key Takeaways](#key-takeaways)

\- \[Skills Practised](#skills-practised)

\- \[AZ-104 Skills Mapping](#az-104-skills-mapping)



\---



\## Why I Built This Lab



As I continue preparing for the AZ-104 certification, I wanted to move beyond resource deployment and focus on governance.



In real-world Azure environments, administrators need to ensure resources are deployed in approved regions, tagged correctly, protected from accidental deletion, and governed consistently across multiple subscriptions.



This lab allowed me to explore how Azure governance services work together to maintain compliance and reduce operational risk.



\---



\## Business Scenario



An organisation wants to standardise Azure resource management across multiple teams and subscriptions.



To achieve this, administrators need to:



\- Restrict deployments to approved Azure regions

\- Enforce resource tagging standards

\- Automatically remediate non-compliant resources

\- Protect critical infrastructure from accidental deletion

\- Organise subscriptions through Management Groups

\- Control permissions through RBAC inheritance

\- Maintain visibility of compliance across the environment



Azure Policy, Resource Locks, Management Groups, and RBAC provide the governance framework needed to support these requirements.



\---



\## What I Wanted to Learn



\- How Azure Policy prevents non-compliant deployments

\- How resource tagging can be enforced through governance controls

\- How policy compliance is monitored and reported

\- How remediation works using Managed Identities

\- How Resource Locks protect Azure resources

\- How Management Groups support governance at scale

\- How RBAC permissions inherit across scopes

\- How Azure resources can be moved between Resource Groups



\---



\## Lab Environment



| Component | Configuration |

|------------|------------|

| Azure Platform | Microsoft Azure |

| Administration Tools | Azure Portal and Azure PowerShell |

| Governance Services | Azure Policy |

| Access Management | Azure RBAC |

| Resource Protection | Resource Locks |

| Organisation Layer | Management Groups |

| Test Resources | Virtual Network and Network Security Group |

| Region | UK South |



\---



\# Implementation



\## Step 1 - Configure the Allowed Locations Policy



\### Objective



I wanted to restrict Azure resource deployments to approved regions.



\### Configuration



I assigned the built-in \*\*Allowed Locations\*\* policy and configured it to allow deployments only within approved Azure locations.



\### Screenshot



<br>



!\[Azure Policy Assignment - Allowed Locations](./Azure%20Policy%20Assignment%20-%20Allowed%20Locations.png)



<br>



!\[Allowed Locations Policy Configuration](./Allowed%20Locations%20Policy%20Configuration.png)



<br>



\### Validation



<br>



I reviewed the policy assignment and confirmed that only approved locations were configured.



\### Result



<br>



The policy became active and enforced deployment restrictions.



\### What I Learned



<br>



Azure Policy can be used to proactively enforce governance controls before resources are deployed.



\---



\## Step 2 - Validate Policy Enforcement



\### Objective



Confirm that the policy actively prevented non-compliant deployments.



\### Configuration



I attempted to deploy a Storage Account in a region that was not permitted by the policy.



\### Screenshot



<br>



!\[Azure Policy Enforcement - Storage Account Deployment Blocked](./Azure%20Policy%20Enforcement%20-%20Storage%20Account%20Deployment%20Blocked.png)



<br>



\### Validation



<br>



Azure evaluated the deployment request against the assigned policy.



\### Result



<br>



The deployment was blocked because it violated the Allowed Locations policy.



\### What I Learned



<br>



Azure Policy can prevent non-compliant resources from being created, reducing governance risks.



\---



\## Step 3 - Configure a Required Resource Tag Policy



\### Objective



Implement mandatory tagging requirements.



\### Configuration



I reviewed the built-in Azure Policy used to require specific tags on resources.



\### Screenshot



<br>



!\[Azure Policy Definition – Require a Tag and Its Value on Resources](./Azure%20Policy%20Definition%20%E2%80%93%20Require%20a%20Tag%20and%20Its%20Value%20on%20Resources.png)



<br>



\### Validation



<br>



I reviewed the policy definition and assignment settings.



\### Result



<br>



Resources could now be assessed for compliance against organisational tagging standards.



\### What I Learned



<br>



Tags are important for governance, reporting, resource management, and cost allocation.



\---



\## Step 4 - Create a Custom Audit Policy



\### Objective



Identify resources that do not meet tagging requirements.



\### Configuration



I created a custom Azure Policy using the Audit effect.



\### Screenshot



<br>



!\[Custom Azure Policy Definition for Resource Tag Auditing](./Custom%20Azure%20Policy%20Definition%20for%20Resource%20Tag%20Auditing.png)



<br>



\### Validation



<br>



The policy evaluated resources against the tagging requirement.



\### Result



<br>



Resources missing required tags were identified as non-compliant.



\### What I Learned



<br>



Audit policies allow administrators to monitor compliance without blocking deployments.



\---



\## Step 5 - Review Policy Compliance



\### Objective



Understand how Azure reports compliance.



\### Configuration



I reviewed the Azure Policy compliance dashboard and detailed compliance reports.



\### Screenshot



<br>



!\[Azure Policy Compliance Dashboard](./Azure%20Policy%20Compliance%20Dashboard%20Showing%20Policy%20Enforcement%20Results.png)



<br>



!\[Policy Compliance Details for Resources Missing Required Environment Tags](./Policy%20Compliance%20Details%20for%20Resources%20Missing%20Required%20Environment%20Tags.png)



<br>



\### Validation



<br>



I reviewed compliance states and investigated policy violations.



\### Result



<br>



Azure clearly identified non-compliant resources and highlighted remediation requirements.



\### What I Learned



<br>



Compliance reporting provides a central view of governance issues across Azure environments.



\---



\## Step 6 - Configure Automatic Tag Remediation



\### Objective



Automatically correct missing resource tags.



\### Configuration



I assigned a policy that inherited tags from the parent Resource Group and configured a Managed Identity for remediation.



\### Screenshot



<br>



!\[Azure Policy Assignment with Managed Identity for Automatic Tag Inheritance](./Azure%20Policy%20Assignment%20with%20Managed%20Identity%20for%20Automatic%20Tag%20Inheritance.png)



<br>



\### Validation



<br>



I confirmed that the policy assignment and Managed Identity were successfully configured.



\### Result



<br>



The environment was prepared for automated remediation.



\### What I Learned



<br>



Managed Identities enable Azure Policy to perform automated remediation without storing credentials.



\---



\## Step 7 - Validate Policy Remediation



\### Objective



Confirm that remediation resolved compliance issues.



\### Configuration



I reviewed compliance results after remediation completed.



\### Screenshot



<br>



!\[Azure Policy Compliance After Automatic Tag Inheritance Remediation](./Azure%20Policy%20Compliance%20After%20Automatic%20Tag%20Inheritance%20Remediation.png)



<br>



\### Validation



<br>



I checked the compliance dashboard following remediation.



\### Result



<br>



The affected resources became compliant and the dashboard reported full compliance.



\### What I Learned



<br>



Azure Policy remediation can automatically resolve compliance issues with minimal administrative effort.



\---



\## Step 8 - Protect Resources Using Resource Locks



\### Objective



Protect critical resources from accidental deletion.



\### Configuration



I applied a CanNotDelete Resource Lock to a Virtual Network.



\### Screenshot



<br>



!\[CanNotDelete Resource Lock Applied to Azure Virtual Network](./CanNotDelete%20Resource%20Lock%20Applied%20to%20Azure%20Virtual%20Network.png)



<br>



!\[Creating a CanNotDelete Resource Lock Using Azure PowerShell](./Creating%20a%20CanNotDelete%20Resource%20Lock%20Using%20Azure%20PowerShell.png)



<br>



\### Validation



<br>



I verified the lock from both the Azure Portal and Azure PowerShell.



\### Result



<br>



The Virtual Network became protected from deletion.



\### What I Learned



<br>



Resource Locks provide an additional layer of protection beyond RBAC permissions.



\---



\## Step 9 - Validate Resource Lock Enforcement



\### Objective



Verify that the lock prevented deletion.



\### Configuration



I attempted to delete the protected Virtual Network.



\### Screenshot



<br>



!\[Resource Deletion Blocked by CanNotDelete Resource Lock](./Resource%20Deletion%20Blocked%20by%20CanNotDelete%20Resource%20Lock.png)



<br>



\### Validation



<br>



Azure evaluated the deletion request against the applied Resource Lock.



\### Result



<br>



The deletion request was successfully blocked.



\### What I Learned



<br>



Resource Locks are highly effective at protecting critical infrastructure.



\---



\## Step 10 - Create and Configure a Management Group



\### Objective



Organise resources through Azure governance hierarchy.



\### Configuration



I created a Management Group and assigned the Azure subscription to it.



\### Screenshot



<br>



!\[Azure Management Group Hierarchy and Subscription Organization](./Azure%20Management%20Group%20Hierarchy%20and%20Subscription%20Organization.png)



<br>



!\[Azure Subscription Assigned to the mg-az104-lab Management Group](./Azure%20Subscription%20Assigned%20to%20the%20mg-az104-lab%20Management%20Groupy:



\- Validated Azure Policy enforcement

\- Tested Policy Compliance reporting

\- Verified automated remediation

\- Confirmed Resource Lock functionality

\- Reviewed RBAC inheritance

\- Validated Management Group configuration

\- Successfully moved resources between Resource Groups



\---



\## Challenges During the Lab



A few areas required additional investigation:



\- Understanding the differences between Audit, Deny, and Modify policy effects

\- Learning how Managed Identities support remediation

\- Understanding policy evaluation and remediation processing

\- Identifying which Azure resources support relocation

\- Understanding the relationship between RBAC permissions and Resource Locks

\- Understanding how Azure Policy can prevent non-compliant deployments before they occur



Working through these challenges improved my understanding of Azure governance and compliance controls.



\---



\## Key Takeaways



\- Azure Policy can prevent and identify non-compliant resources

\- Compliance monitoring is a critical governance capability

\- Managed Identities support secure remediation

\- Resource Locks help protect critical infrastructure

\- Management Groups simplify governance at scale

\- RBAC inheritance reduces administration overhead

\- Resource relocation requires planning and validation

\- Governance controls are most effective when applied proactively



\---



\## Skills Practised



\### Azure Governance



\- Azure Policy

\- Policy Compliance Monitoring

\- Policy Remediation

\- Resource Tag Governance



\### Security and Access Control



\- Azure RBAC

\- RBAC Inheritance

\- Resource Locks

\- Management Groups



\### Azure Administration



\- Azure Resource Manager

\- Resource Group Administration

\- Resource Relocation

\- Azure PowerShell



\### Operations



\- Governance Reporting

\- Compliance Validation

\- Resource Lifecycle Management



\---



\## AZ-104 Skills Mapping



This lab aligns with the following AZ-104 objectives:



\- Manage Azure subscriptions and governance

\- Configure and manage Azure Policy

\- Configure Resource Locks

\- Configure Azure Management Groups

\- Configure Role-Based Access Control (RBAC)

\- Manage Resource Groups

\- Move Azure resources

\- Monitor and remediate compliance issues

