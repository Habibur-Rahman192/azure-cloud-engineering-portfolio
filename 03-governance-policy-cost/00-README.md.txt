# AZ-104 Lab 03: Azure Governance and Resource Management

## Overview

In this lab, I implemented Azure governance and resource management controls across a cloud environment using Azure Policy, Resource Locks, Management Groups, RBAC, and Azure Resource Manager.

The project focused on enforcing compliance, protecting critical resources, automating governance tasks, and managing access through enterprise-grade Azure governance solutions.

By the end of the lab, I successfully achieved full policy compliance, implemented automated tag remediation, protected network resources with Resource Locks, and centralized governance through Management Groups and RBAC inheritance.
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

![Figure 4](images/policy-assignment.png)

---

## Figure 5: Azure Policy Enforcement - Storage Account Deployment Blocked

Validated policy enforcement by attempting to deploy a storage account in a region that was not permitted by policy.

![Figure 5](images/policy-enforcement-blocked.png)

---

## Figure 6: Allowed Locations Policy Configuration (UK South Enforcement)

Configured policy parameters to ensure resources could only be deployed within **UK South**.

![Figure 6](images/allowed-locations-configuration.png)

---

## Figure 7: Azure Policy Definition – Require a Tag and Its Value on Resources

Reviewed the built-in policy used to enforce mandatory resource tagging standards.

![Figure 7](images/tag-policy-definition.png)

---

## Figure 8: Custom Azure Policy Definition for Resource Tag Auditing

Created a custom policy to audit resources that did not contain the required tag.

![Figure 8](images/custom-tag-audit-policy.png)

---

## Figure 9: Azure Policy Compliance Dashboard

Reviewed compliance results and identified resources that met or failed policy requirements.

![Figure 9](images/compliance-dashboard.png)

---

## Figure 10: Policy Compliance Details for Resources Missing Required Environment Tags

Investigated policy audit results and reviewed resource-level compliance findings.

![Figure 10](images/tag-compliance-details.png)

---

# Azure Policy Remediation

## Figure 16: Policy Assignment with Managed Identity for Automatic Tag Inheritance

Configured a system-assigned Managed Identity to automatically inherit tags from the parent Resource Group.

![Figure 16](images/tag-remediation-managed-identity.png)

---

## Figure 20: Compliance After Remediation

Validated successful remediation and confirmed 100% compliance across all evaluated resources.

![Figure 20](images/post-remediation-compliance.png)

---

# Resource Protection

## Figure 11: CanNotDelete Resource Lock Applied to Azure Virtual Network

Applied a Resource Lock to protect critical network infrastructure from accidental deletion.

![Figure 11](images/resource-lock.png)

---

## Figure 12: Resource Deletion Blocked by Resource Lock

Validated Resource Lock enforcement by attempting a deletion operation.

![Figure 12](images/resource-lock-validation.png)

---

## Figure 19: Creating a Resource Lock with Azure PowerShell

Created a Resource Lock through Azure PowerShell automation.

```powershell
New-AzResourceLock `
    -LockName "lock-vnet-az104-lab" `
    -LockLevel CanNotDelete `
    -ResourceGroupName "rg-az104-lab-uksouth"
```

![Figure 19](images/powershell-lock-creation.png)

---

# Azure Management Groups

## Figure 13: Azure Management Group Hierarchy and Subscription Organization

Created a Management Group hierarchy to organize Azure subscriptions and apply governance consistently.

![Figure 13](images/management-group-overview.png)

---

## Figure 14: Azure Subscription Assigned to the Management Group

Moved the Azure subscription into the custom Management Group to support governance and policy inheritance.

![Figure 14](images/subscription-management-group.png)

---

# Azure Role-Based Access Control (RBAC)

## Figure 15: RBAC Owner Role Assignment and Management Group Inheritance

Reviewed Owner role assignments and verified inherited permissions from the Management Group scope.

Key concepts demonstrated:

- Direct role assignments
- Role inheritance
- Subscription-level access control
- Management Group location process.

images/resource-move-review.png

---

## Figure 17: Network Security Group Successfully Moved

Successfully relocated a Network Security Group between Resource Groups using Azure Resource Manager.

images/nsg-move-success.png

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

Successfully implemented governance controls across an Azure environment using Azure Policy, Resource Locks, Management Groups, RBAC, and Azure Resource Manager.

The environment achieved full compliance following remediation activities and demonstrated key enterprise governance capabilities, including policy enforcement, automated tagging, access control inheritance, resource protection, and controlled resource relocation.

This lab provided practical experience with core governance and resource management concepts required for the **Microsoft Azure Administrator (AZ-104)** certification.

---
