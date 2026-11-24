# AZ-104 Lab 03: Azure Governance and Resource Management

## Overview

In this lab, I implemented Azure governance and resource management controls across a cloud environment using Azure Policy, Resource Locks, Management Groups, RBAC, and Azure Resource Manager.

The project focused on enforcing compliance, protecting critical resources, automating governance tasks, and managing access through enterprise-grade Azure governance solutions.

By the end of the lab, I successfully achieved full policy compliance, implemented automated tag remediation, protected network resources with Resource Locks, and centralized governance through Management Groups and RBAC inheritance.

---

## Business Scenario

As Azure environments grow, governance becomes increasingly important for maintaining security, compliance, and operational consistency.

This lab simulates an enterprise environment where an Azure Administrator must:

- Restrict deployments to approved Azure regions
- Enforce organizational tagging standards
- Protect critical resources from accidental deletion
- Manage permissions using RBAC
- Organize subscriptions using Management Groups
- Maintain compliance through automated remediation

These controls help ensure that Azure resources remain compliant with organizational policies while reducing administrative overhead.

---

## Technologies Used

- Microsoft Azure Portal
- Azure PowerShell
- Azure Resource Manager (ARM)
- Azure Policy
- Azure Management Groups
- Azure Role-Based Access Control (RBAC)
- Azure Resource Locks
- Network Security Groups (NSGs)

---

## Lab Objectives

- Configure and assign Azure Policies
- Enforce deployment restrictions using policy controls
- Implement resource tagging governance
- Monitor policy compliance
- Configure policy remediation using Managed Identity
- Protect resources using Resource Locks
- Organize subscriptions using Management Groups
- Review RBAC inheritance and permissions
- Move resources between Resource Groups
- Manage governance controls using Azure PowerShell

---

# Azure Policy Governance

## Figure 4: Azure Policy Assignment - Allowed Locations

Assigned the built-in **Allowed Locations** policy to restrict resource deployment to approved Azure regions.

![Figure 4](Azure%20Policy%20Assignment%20-%20Allowed%20Locations.png)

---

## Figure 5: Azure Policy Enforcement - Storage Account Deployment Blocked

Validated policy enforcement by attempting to deploy a storage account in a region that was not permitted by policy.

![Figure 5](Azure%20Policy%20Enforcement%20-%20Storage%20Account%20Deployment%20Blocked.png)

---

## Figure 6: Allowed Locations Policy Configuration (UK South Enforcement)

Configured policy parameters to ensure resources could only be deployed within **UK South**.

![Figure 6](Allowed%20Locations%20Policy%20Configuration.png)

---

## Figure 7: Azure Policy Definition – Require a Tag and Its Value on Resources

Reviewed the built-in policy used to enforce mandatory resource tagging standards.

![Figure 7](Azure%20Policy%20Definition%20-%20Require%20a%20Tag%20and%20Its%20Value%20on%20Resources.png)

---

## Figure 8: Custom Azure Policy Definition for Resource Tag Auditing

Created a custom policy to audit resources that did not contain the required tag.

![Figure 8](Custom%20Azure%20Policy%20Definition%20for%20Resource%20Tag%20Auditing.png)

---

## Figure 9: Azure Policy Compliance Dashboard

Reviewed compliance results and identified resources that met or failed policy requirements.

![Figure 9](Azure%20Policy%20Compliance%20Dashboard%20Showing%20Policy%20Enforcement%20Results.png)

---

## Figure 10: Policy Compliance Details for Resources Missing Required Environment Tags

Investigated policy audit results and reviewed resource-level compliance findings.

![Figure 10](Policy%20Compliance%20Details%20for%20Resources%20Missing%20Required%20Environment%20Tags.png)

---

# Azure Policy Remediation

## Figure 16: Policy Assignment with Managed Identity for Automatic Tag Inheritance

Configured a system-assigned Managed Identity to automatically inherit tags from the parent Resource Group.

![Figure 16](Azure%20Policy%20Assignment%20with%20Managed%20Identity%20for%20Automatic%20Tag%20Inheritance.png)

---

## Figure 20: Compliance After Remediation

Validated successful remediation and confirmed 100% compliance across all evaluated resources.

![Figure 20](Azure%20Policy%20Compliance%20After%20Automatic%20Tag%20Inheritance%20Remediation.png)

---

# Resource Protection

## Figure 11: CanNotDelete Resource Lock Applied to Azure Virtual Network

Applied a Resource Lock to protect critical network infrastructure from accidental deletion.

![Figure 11](CanNotDelete%20Resource%20Lock%20Applied%20to%20Azure%20Virtual%20Network.png)

---

## Figure 12: Resource Deletion Blocked by Resource Lock

Validated Resource Lock enforcement by attempting a deletion operation.

![Figure 12](Resource%20Deletion%20Blocked%20by%20CanNotDelete%20Resource%20Lock.png)

---

## Figure 19: Creating a Resource Lock with Azure PowerShell

Created a Resource Lock through Azure PowerShell automation.

```powershell
New-AzResourceLock `
    -LockName "lock-vnet-az104-lab" `
    -LockLevel CanNotDelete `
    -ResourceGroupName "rg-az104-lab-uksouth"
```

![Figure 19](Creating%20a%20CanNotDelete%20Resource%20Lock%20Using%20Azure%20PowerShell.png)

---

# Azure Management Groups

## Figure 13: Azure Management Group Hierarchy and Subscription Organization

Created a Management Group hierarchy to organize Azure subscriptions and apply governance consistently.

![Figure 13](Azure%20Management%20Group%20Hierarchy%20and%20Subscription%20Organization.png)

---

## Figure 14: Azure Subscription Assigned to the Management Group

Moved the Azure subscription into the custom Management Group to support governance and policy inheritance.

![Figure 14](Azure%20Subscription%20Assigned%20to%20the%20mg-az104-lab%20Management%20Group.png)

---

# Azure Role-Based Access Control (RBAC)

## Figure 15: RBAC Owner Role Assignment and Management Group Inheritance

Reviewed Owner role assignments and verified inherited permissions from the Management Group scope.

Key concepts demonstrated:

- Direct role assignments
- Role inheritance
- Subscription-level access control
- Management Group governance

![Figure 15](Screenshot%202026-11-22%20100503.png)

---

# Azure Resource Management

## Figure 18: Resource Move Review

Reviewed and validated the source and destination Resource Groups before initiating the move operation.

![Figure 18](Azure%20Resource%20Move%20Review%20Showing%20Source%20and%20Target%20Resource%20Groups.png)

---

## Figure 17: Network Security Group Successfully Moved

Successfully relocated a Network Security Group between Resource Groups using Azure Resource Manager.

![Figure 17](Network%20Security%20Group%20Successfully%20Moved%20to%20the%20Target%20Resource%20Group.png)

---

# PowerShell Commands Used

### Connect to Azure

```powershell
Connect-AzAccount
```

### View Tenant Information

```powershell
Get-AzTenant
```

### Review Current Context

```powershell
Get-AzContext
```

### Create Resource Locks

```powershell
New-AzResourceLock `
    -LockName "lock-vnet-az104-lab" `
    -LockLevel CanNotDelete `
    -ResourceGroupName "rg-az104-lab-uksouth"
```

### Verify Resource Locks

```powershell
Get-AzResourceLock
```

### Review Policy Compliance

```powershell
Get-AzPolicyState
```

### View Resource Groups

```powershell
Get-AzResourceGroup
```

---

# Project Results

✅ 100% Azure Policy Compliance Achieved

✅ Automated Resource Tag Remediation Configured

✅ Resource Deletion Protection Implemented

✅ Management Group Hierarchy Created

✅ RBAC Inheritance Validated

✅ Azure Resource Relocation Completed

✅ Governance Controls Successfully Implemented

---

# Challenges and Lessons Learned

Throughout this lab, several important governance concepts became clear:

- Policy assignments alone do not automatically remediate existing resources.
- Managed Identities are required when using Modify policies for remediation.
- Resource Locks prevent accidental deletion but do not restrict read access.
-  deployments before they occur.

---

# Key Takeaways

- Azure Policy enables centralized governance and compliance enforcement.
- Allowed Locations policies restrict deployments to approved Azure regions.
- Resource tagging improves governance, reporting, and operational consistency.
- Policy remediation can automatically correct non-compliant resources.
- Resource Locks help prevent accidental deletion of critical infrastructure.
- Management Groups provide hierarchical governance across subscriptions.
- RBAC enables secure and scalable access management.
- Azure Resource Manager supports resource relocation across Resource Groups.
- Azure PowerShell simplifies Azure administration and automation.

---

# Skills Demonstrated

✅ Azure Policy Assignment and Enforcement

✅ Azure Policy Compliance Monitoring

✅ Policy Remediation and Managed Identity

✅ Resource Tag Governance

✅ Resource Locks

✅ Azure Management Groups

✅ Azure RBAC

✅ Azure PowerShell Administration

✅ Resource Group Management

✅ Azure Resource Relocation

✅ Governance and Compliance Best Practices

---

# AZ-104 Skills Mapping

This lab aligns with the following AZ-104 objectives:

- Manage Azure subscriptions and governance
- Configure and manage Azure Policy
- Configure Resource Locks
- Configure Management Groups
- Configure Role-Based Access Control (RBAC)
- Manage Resource Groups
- Move Azure resources
- Monitor and remediate compliance issues

---

# Lab Outcome

Successfully designed and implemented an Azure governance framework using Azure Policy, Resource Locks, Management Groups, RBAC, and Azure Resource Manager.

Key achievements included:

- Restricting resource deployments to approved Azure regions
- Enforcing organizational tagging standards
- Implementing automated tag remediation using Managed Identity
- Achieving 100% policy compliance
- Protecting critical resources with Resource Locks
- Centralizing governance through Management Groups
- Validating RBAC inheritance and access controls
- Successfully relocating Azure resources between Resource Groups

