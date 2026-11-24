# AZ-104 Lab 03: Azure Governance, Policy and Resource Management

## Overview

In this lab, I focused on Azure governance and resource management by working with Azure Policy, Resource Locks, Management Groups, RBAC inheritance, and resource relocation.

Rather than simply creating resources, I wanted to understand how Azure administrators enforce standards, protect resources, and manage compliance across an Azure environment. This lab gave me practical experience with some of the governance and control mechanisms that are commonly used in enterprise Azure deployments.

\---

# Why I Built This Lab

As I continue working towards the AZ-104 certification, I wanted to move beyond deploying resources and focus on governance.

Most production Azure environments require controls to ensure that resources are deployed correctly, tagged appropriately, protected from accidental deletion, and managed consistently across subscriptions.

The goal of this lab was to explore how Azure provides governance at scale and to understand how administrators can implement policies that support operational, security, and compliance requirements.

\---

# Business Scenario

An organisation wants to standardise Azure resource management across multiple teams and subscriptions.

To achieve this, administrators need to:

* Restrict deployments to approved Azure regions
* Enforce mandatory resource tags
* Automatically remediate non-compliant resources
* Protect critical infrastructure from accidental deletion
* Organise subscriptions through Management Groups
* Control permissions through RBAC inheritance
* Maintain visibility of compliance across the environment

Azure Policy, Resource Locks, Management Groups, and RBAC provide the building blocks required to implement these controls.

\---

# What I Wanted to Learn

* How Azure Policy prevents non-compliant deployments
* How resource tagging can be enforced through governance controls
* How policy compliance is monitored and reported
* How remediation tasks work using Managed Identities
* How Resource Locks protect Azure resources
* How Management Groups support governance across subscriptions
* How RBAC permissions inherit across scopes
* How Azure resources can be moved between Resource Groups

\---

# Lab Environment

|Component|Configuration|
|-|-|
|Azure Platform|Microsoft Azure|
|Administration Tools|Azure Portal, Azure PowerShell|
|Governance Services|Azure Policy|
|Access Management|Azure RBAC|
|Resource Protection|Resource Locks|
|Organisation Layer|Management Groups|
|Test Resources|Virtual Network, Network Security Group|
|Region|UK South|

\---

# Implementation

## Step 1 - Configure the Allowed Locations Policy

### Objective

I wanted to restrict Azure resource deployments to approved regions.

### Configuration

I assigned the built-in **Allowed Locations** policy to the Resource Group scope and configured the policy to allow deployments only within the approved Azure region.

### Evidence

!\[Figure 4 - Azure Policy Assignment - Allowed Locations](InvalidUrl)

!\[Figure 6 - Allowed Locations Policy Configuration](InvalidUrl)

### Validation

I reviewed the policy assignment and confirmed that the correct location restrictions were configured.

### Result

The policy became active and prevented deployments outside the approved region.

### What I Learned

Azure Policy can be used to proactively enforce governance standards before resources are deployed.

\---

## Step 2 - Validate Policy Enforcement

### Objective

Confirm that the Allowed Locations policy was actively enforcing restrictions.

### Configuration

I attempted to deploy a Storage Account in a region that was not permitted by the policy.

### Evidence

!\[Figure 5 - Azure Policy Enforcement - Storage Account Deployment Blocked](InvalidUrl)

### Validation

Azure evaluated the deployment request against the assigned policy.

### Result

The deployment was blocked because it violated the policy configuration.

### What I Learned

Policy enforcement occurs before deployment completes, reducing the risk of non-compliant resources being created.

\---

## Step 3 - Configure a Required Resource Tag Policy

### Objective

Enforce organisational tagging standards.

### Configuration

I reviewed and assigned a policy requiring resources to contain specific tags.

### Evidence

!\[Figure 7 - Azure Policy Definition – Require a Tag and Its Value on Resources](InvalidUrl)

### Validation

I reviewed the policy definition and assignment settings.

### Result

Resources could be assessed against the policy to determine tag compliance.

### What I Learned

Resource tags play an important role in governance, reporting, automation, and operational management.

\---

## Step 4 - Create a Custom Audit Policy

### Objective

Audit resources missing required tags.

### Configuration

I created a custom Azure Policy definition with an Audit effect.

### Evidence

!\[Figure 8 - Custom Azure Policy Definition for Resource Tag Auditing](InvalidUrl)

### Validation

The policy evaluated resources against the tagging requirement.

### Result

Azure identified resources that did not meet the required tagging standard.

### What I Learned

Audit policies provide visibility into compliance without preventing deployments.

\---

## Step 5 - Review Policy Compliance

### Objective

Understand how Azure reports policy compliance.

### Configuration

I reviewed the Azure Policy Compliance dashboard and compliance reports.

### Evidence

!\[Figure 9 - Azure Policy Compliance Dashboard](InvalidUrl)

!\[Figure 10 - Policy Compliance Details for Resources Missing Required Environment Tags](InvalidUrl)

### Validation

I reviewed compliance states and investigated non-compliant resources.

### Result

Azure clearly identified policy violations and highlighted resources requiring remediation.

### What I Learned

Compliance reporting provides administrators with a central view of governance issues.

\---

## Step 6 - Configure Automatic Tag Remediation

### Objective

Automatically apply tags to resources missing required values.

### Configuration

I assigned a policy that inherited tags from the Resource Group and configured a Managed Identity to perform remediation.

### Evidence

!\[Figure 16 - Azure Policy Assignment with Managed Identity for Automatic Tag Inheritance](InvalidUrl)

### Validation

The policy assignment and remediation identity were successfully configured.

### Result

The environment was prepared to automatically remediate resources missing required tags.

### What I Learned

Managed Identities allow Azure Policy to perform remediation tasks securely without requiring stored credentials.

\---

## Step 7 - Validate Policy Remediation

### Objective

Confirm that remediation resolved compliance issues.

### Configuration

I reviewed the compliance results after remediation was completed.

### Evidence

!\[Figure 20 - Azure Policy Compliance After Automatic Tag Inheritance Remediation](InvalidUrl)

### Validation

I checked the compliance dashboard after the remediation process completed.

### Result

Compliance increased to 100% for the evaluated resources.

### What I Learned

Azure Policy remediation can automatically bring resources back into compliance without manual intervention.

\---

## Step 8 - Protect Resources Using Resource Locks

### Objective

Prevent accidental deletion of critical resources.

### Configuration

I applied a **CanNotDelete** Resource Lock to a Virtual Network.

### Evidence

!\[Figure 11 - CanNotDelete Resource Lock Applied to Azure Virtual Network](InvalidUrl)

!\[Figure 19 - Creating a CanNotDelete Resource Lock Using Azure PowerShell](InvalidUrl)

### Validation

The lock assignment was reviewed in the Azure Portal and through PowerShell.

### Result

The Virtual Network became protected from accidental deletion.

### What I Learned

Resource Locks provide an additional layer of protection beyond RBAC permissions.

\---

## Step 9 - Validate Resource Lock Enforcement

### Objective

Verify that the lock was functioning correctly.

### Configuration

I attempted to delete the protected resource.

### Evidence

!\[Figure 12 - Resource Deletion Blocked by CanNotDelete Resource Lock](InvalidUrl)

### Validation

Azure evaluated the deletion request against the applied lock.

### Result

The deletion was blocked successfully.

### What I Learned

Resource Locks are highly effective at preventing accidental deletion of critical infrastructure.

\---

## Step 10 - Create and Configure a Management Group

### Objective

Organise subscriptions using Azure governance hierarchy.

### Configuration

I created a Management Group and moved the Azure subscription into it.

### Evidence

!\[Figure 13 - Azure Management Group Hierarchy and Subscription Organization](InvalidUrl)

!\[Figure 14 - Azure Subscription Assigned to the Management Group](InvalidUrl)

### Validation

I reviewed the Management Group structure and subscription placement.

### Result

The subscription was successfully managed within the governance hierarchy.

### What I Learned

Management Groups provide a scalable way to organise subscriptions and apply governance controls consistently.

\---

## Step 11 - Review RBAC Inheritance

### Objective

Understand how permissions inherit from Management Groups.

### Configuration

I reviewed role assignments and inherited permissions.

### Evidence

!\[Figure 15 - RBAC Owner Role Assignment and Management Group Inheritance](InvalidUrl)

### Validation

I reviewed role assignment scope and inheritance details.

### Result

Permissions assigned at higher scopes were inherited by child resources.

### What I Learned

RBAC inheritance simplifies administrative management and helps ensure consistency across environments.

\---

## Step 12 - Move Resources Between Resource Groups

### Objective

Understand Azure resource relocation capabilities.

### Configuration

I moved a Network Security Group between Resource Groups.

### Evidence

!\[Figure 18 - Azure Resource Move Review](InvalidUrl)

!\[Figure 17 - Network Security Group Successfully Moved](InvalidUrl)

### Validation

I reviewed the move operation and confirmed the resource location after completion.

### Result

The Network Security Group was successfully relocated.

### What I Learned

Azure Resource Manager supports resource movement between Resource Groups when dependency requirements are met.

\---

# Validation and Testing

The following activities were completed throughout the lab:

* Verified policy assignment and enforcement
* Tested policy compliance reporting
* Validated automatic remediation functionality
* Confirmed Resource Lock enforcement
* Reviewed RBAC inheritance
* Confirmed Management Group hierarchy configuration
* Successfully moved resources between Resource Groups

\---

# Challenges During the Lab

A few areas required additional investigation:

* Understanding the differences between Audit, Deny, and Modify policy effects.
* Learning how Managed Identities support policy remediation.
* Understanding how compliance evaluations and remediation tasks are processed.
* Identifying which Azure resources can be moved between Resource Groups and any associated limitations.
* Understanding the relationship between RBAC permissions and Resource Locks.

Working through these challenges gave me a much better understanding of how governance controls operate in Azure environments.

\---

# Key Takeaways

* Azure Policy can both prevent and identify non-compliant resources.
* Compliance monitoring is a critical part of governance.
* Managed Identities enable secure policy remediation.
* Resource Locks help protect critical infrastructure.
* Management Groups simplify governance across subscriptions.
* RBAC inheritance reduces administrative complexity.
* Resource relocation requires validation of dependencies and supported resource types.
* Governance controls are most effective when implemented proactively rather than reactively.

\---

# Skills Practised

### Azure Governance

* Azure Policy
* Policy Compliance Monitoring
* Policy Remediation
* Resource Tag Governance

### Security and Access Control

* Azure RBAC
* RBAC Inheritance
* Resource Protection
* Management Groups

### Azure Administration

* Azure Resource Manager
* Resource Group Administration
* Resource Relocation
* Azure PowerShell

### Operations

* Compliance Validation
* Governance Reporting
* Resource Lifecycle Management

\---

# AZ-104 Skills Mapping

This lab aligns with the following AZ-104 objectives:

* Manage Azure subscriptions and governance
* Configure and manage Azure Policy
* Configure Resource Locks
* Configure Azure Management Groups
* Configure Role-Based Access Control (RBAC)
* Manage Resource Groups
* Move Azure resources
* Monitor and remediate compliance issues

