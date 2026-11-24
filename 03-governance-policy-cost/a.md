\# AZ-104 Lab 03: Azure Governance, Policy and Resource Management



\## Overview



In this lab, I focused on Azure governance and resource management by implementing Azure Policy, Resource Locks, Management Groups, RBAC inheritance, and resource relocation.



Rather than simply deploying resources, I wanted to understand how Azure administrators enforce standards, protect critical infrastructure, and maintain compliance across an Azure environment. This lab gave me hands-on experience with several governance capabilities that are commonly used in enterprise Azure deployments.



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



This lab allowed me to explore how Azure governance features work together to help organisations maintain compliance and reduce operational risk.



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

\- How governance controls enforce tagging standards

\- How policy compliance is monitored and reported

\- How remediation tasks work using Managed Identities

\- How Resource Locks protect Azure resources

\- How Management Groups support governance at scale

\- How RBAC permissions inherit across scopes

\- How Azure resources can be moved between Resource Groups



\---



\## Lab Environment



| Component | Configuration |

|------------|------------|

| Azure Platform | Microsoft Azure |

| Administration Tools | Azure Portal, Azure PowerShell |

| Governance Services | Azure Policy |

| Access Management | Azure RBAC |

| Resource Protection | Resource Locks |

| Organisation Layer | Management Groups |

| Test Resources | Virtual Network, Network Security Group |

| Region | UK South |



\---



\# Implementation



\## Step 1 - Configure the Allowed Locations Policy



\### Objective



Restrict deployments to approved Azure regions.



\### Configuration



I assigned the built-in \*\*Allowed Locations\*\* policy and configured it to allow deployments only within approved regions.



\### Evidence



!\[Figure 4 - Azure Policy Assignment - Allowed Locations](Azure%20Policy%20Assignment%20-%20Allowed%20Locations.png)



!\[Figure 6 - Allowed Locations Policy Configuration](Allowed%20Locations%20Policy%20Configuration.png)



\### Result



The policy was assigned successfully and became active.



\### What I Learned



Azure Policy can enforce governance before resources are deployed.



\---



\## Step 2 - Validate Policy Enforcement



\### Objective



Confirm that the policy actively blocks unauthorised deployments.



\### Evidence



!\[Figure 5 - Azure Policy Enforcement - Storage Account Deployment Blocked](Azure%20Policy%20Enforcement%20-%20Storage%20Account%20Deployment%20Blocked.png)



\### Validation



I attempted to create a Storage Account in a blocked region.



\### Result



Azure denied the deployment due to the policy assignment.



\### What I Learned



Preventative controls are often more effective than identifying issues after deployment.



\---



\## Step 3 - Configure a Required Resource Tag Policy



!\[Figure 7 - Azure Policy Definition – Require a Tag and Its Value on Resources](Azure%20Policy%20Definition%20%E2%80%93%20Require%20a%20Tag%20and%20Its%20Value%20on%20Resources.png)



\### What I Learned



Tags are important for governance, reporting, automation, and cost allocation.



\---



\## Step 4 - Create a Custom Audit Policy



!\[Figure 8 - Custom Azure Policy Definition for Resource Tag Auditing](Custom%20Azure%20Policy%20Definition%20for%20Resource%20Tag%20Auditing.png)



\### What I Learned



Audit policies identify compliance issues without preventing deployments.



\---



\## Step 5 - Review Policy Compliance



!\[Figure 9 - Azure Policy Compliance Dashboard](Azure%20Policy%20Compliance%20Dashboard%20Showing%20Policy%20Enforcement%20Results.png)



!\[Figure 10 - Policy Compliance Details for Resources Missing Required Environment Tags](Policy%20Compliance%20Details%20for%20Resources%20Missing%20Required%20Environment%20Tags.png)



\### What I Learned



Compliance reports provide visibility into governance issues across Azure resources.



\---



\## Step 6 - Configure Automatic Tag Remediation



!\[Figure 16 - Azure Policy Assignment with Managed Identity for Automatic Tag Inheritance](Azure%20Policy%20Assignment%20with%20Managed%20Identity%20for%20Automatic%20Tag%20Inheritance.png)



\### What I Learned



Managed Identities enable Azure Policy to remediate resources securely.



\---



\## Step 7 - Validate Policy Remediation



!\[Figure 20 - Azure Policy Compliance After Automatic Tag Inheritance Remediation](Azure%20Policy%20Compliance%20After%20Automatic%20Tag%20Inheritance%20Remediation.png)



\### Result



Compliance improved to 100% for the evaluated resources.



\---



\## Step 8 - Protect Resources Using Resource Locks



!\[Figure 11 - CanNotDelete Resource Lock Applied to Azure Virtual Network](CanNotDelete%20Resource%20Lock%20Applied%20to%20Azure%20Virtual%20Network.png)



!\[Figure 19 - Creating a CanNotDelete Resource Lock Using Azure PowerShell](Creating%20a%20CanNotDelete%20Resource%20Lock%20Using%20Azure%20PowerShell.png)



\### What I Learned



Resource Locks provide protection even when users have permission to manage resources.



\---



\## Step 9 - Validate Resource Lock Enforcement



!\[Figure 12 - Resource Deletion Blocked by CanNotDelete Resource Lock](Resource%20Deletion%20Blocked%20by%20CanNotDelete%20Resource%20Lock.png)



\### Result



The deletion operation was successfully blocked.



\---



\## Step 10 - Create and Configure a Management Group



!\[Figure 13 - Azure Management Group Hierarchy and Subscription Organization](Azure%20Management%20Group%20Hierarchy%20and%20Subscription%20Organization.png)



!\[Figure 14 - Azure Subscription Assigned to the mg-az104-lab Management Group](Azure%20Subscription%20Assigned%20to%20the%20mg-az104-lab%20Management%20Group.png)



\### What I Learned



Management Groups simplify governance and policy inheritance across subscriptions.



\---



\## Step 11 - Review RBAC Inheritance



!\[Figure 15 - RBAC Owner Role Assignment and Management Group Inheritance](Screenshot%202026-11-22%20100503.png)



\### What I Learned



Permissions assigned at higher scopes can be inherited by child resources.



\---



\## Step 12 - Move Resources Between Resource Groups



!\[Figure 18 - Azure Resource Move Review](Azure%20Resource%20Move%20Review%20Showing%20Source%20and%20Target%20Resource%20Groups.png)



!\[Figure 17 - Network Security Group Successfully Moved](Network%20Security%20Group%20Successfully%20Moved%20to%20the%20Target%20Resource%20Group.png)



\### What I Learned



Azure Resource Manager supports resource movement when dependencies and resource types allow it.



\---



\## Validation and Testing



During this lab I successfully:



\- Validated Azure Policy enforcement

\- Tested compliance reporting

\- Verified automatic remediation

\- Confirmed Resource Lock functionality

\- Reviewed RBAC inheritance

\- Validated Management Group configuration

\- Successfully moved resources between Resource Groups



\---



\## Challenges During the Lab



A few areas required additional investigation:



\- Understanding the differences between Audit, Deny and Modify policy effects

\- Learning how Managed Identities support remediation

\- Understanding compliance evaluation and remediation processing

\- Identifying which Azure resources support relocation

\- Understanding how Resource Locks interact with RBAC permissions

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



\- Azure Resource 

