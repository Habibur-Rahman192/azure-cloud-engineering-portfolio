# Azure RBAC with Microsoft Entra ID Security Groups

> A hands-on Azure lab where I explored how Azure Role-Based Access Control (RBAC) works in practice by assigning permissions through Microsoft Entra ID security groups, validating access, and testing access removal.

---

## 📑 Table of Contents

- [Why I Built This Lab](#-why-i-built-this-lab)
- [What I Wanted to Learn](#-what-i-wanted-to-learn)
- [Lab Environment](#-lab-environment)
- [Architecture](#-architecture)
- [Implementation](#-implementation)
  - [Step 1 - Create a Guest User](#step-1---create-a-guest-user)
  - [Step 2 - Create a Security Group](#step-2---create-a-security-group)
  - [Step 3 - Assign the Reader Role](#step-3---assign-the-reader-role)
  - [Step 4 - Verify Permission Inheritance](#step-4---verify-permission-inheritance)
  - [Step 5 - Review the Reader Role Definition](#step-5---review-the-reader-role-definition)
  - [Step 6 - Test Permission Restrictions](#step-6---test-permission-restrictions)
  - [Step 7 - Explore Additional RBAC Roles](#step-7---explore-additional-rbac-roles)
  - [Step 8 - Remove the Reader Role Assignment](#step-8---remove-the-reader-role-assignment)
  - [Step 9 - Verify Access Revocation](#step-9---verify-access-revocation)
- [Challenges During the Lab](#-challenges-during-the-lab)
- [Key Takeaways](#-key-takeaways)
- [Skills Practiced](#-skills-practiced)
- [Next Steps](#-next-steps)
- [Author](#-author)

---

# 📖 Why I Built This Lab

While studying Azure administration and preparing for the AZ-104 certification, I wanted to move beyond reading documentation and actually see how Azure RBAC behaves in a practical environment.

I often came across Microsoft's recommendation to manage permissions through security groups rather than assigning roles directly to users. I understood the theory, but I wanted to build a small environment and test the entire process myself.

The goal was simple:

> Give a user read-only access to Azure resources, verify what they can and cannot do, and then confirm that access disappears when the role assignment is removed.

This lab helped me better understand how Azure administrators manage permissions at scale while following security best practices.

---

# 🎯 What I Wanted to Learn

- How Azure RBAC works in practice
- How Microsoft Entra ID security groups are used for authorization
- How role inheritance works through group membership
- What permissions the Reader role actually grants
- How permission revocation behaves in Azure
- Why group-based access management is recommended

---

# 🛠 Lab Environment

| Component | Configuration |
|------------|------------|
| Azure Service | Azure RBAC |
| Identity Provider | Microsoft Entra ID |
| User Type | Guest User |
| Security Group | `sg-az104-lab-readers` |
| Azure Role | Reader |
| Scope | Resource Group |
| Test Method | Azure Portal |

---

# 🏗 Architecture

The environment was intentionally kept simple so I could focus on understanding RBAC permission inheritance.

```text
Guest User
      │
      ▼
Microsoft Entra ID Security Group
      │
      ▼
Azure RBAC Reader Role
      │
      ▼
Resource Group
      │
      ▼
Azure Resources
```

Instead of assigning permissions directly to a user account, the Reader role was assigned to a security group. The user then received access through group membership.

This is the same approach commonly used in production environments because it is easier to manage, audit, and scale.

---

# 🚀 Implementation

## Step 1 - Create a Guest User

I started by creating a guest account in Microsoft Entra ID to simulate an external user who needed access to Azure resources.

### Screenshot

![Guest User Added](./Guest%20user%20has%20been%20added%20to%20the%20tenant.png)

### What I Learned

Guest users can be granted Azure permissions in exactly the same way as internal users through security groups and RBAC assignments.

---

## Step 2 - Create a Security Group

Next, I created a dedicated security group:

```text
sg-az104-lab-readers
```

Rather than assigning permissions directly to a user, I wanted to follow Microsoft's recommended approach and use group-based access control.

### Why This Matters

Managing permissions through security groups makes administration significantly easier.

Instead of updating role assignments every time a user joins or leaves a team, administrators can simply update group membership.

---

## Step 3 - Assign the Reader Role

I assigned the built-in Azure Reader role to the security group at the Resource Group scope.

### Screenshot

![Reader Role Assignment](./Azure%20RBAC%20Reader%20Role%20Assignment%20for%20Lab%20Users.png)

### Result

The group was granted permission to:

✅ View resources

✅ Review resource configurations

✅ View resource information

The group could not:

❌ Create resources

❌ Modify resources

❌ Delete resources

❌ Manage permissions

---

## Step 4 - Verify Permission Inheritance

After adding the guest account to the security group, I checked the user's effective permissions.

### Screenshot

![Permission Inheritance](./lab-reader-01%20has%20a%20Reader%20role%20assignment%20on%20the%20Azure%20resource%20group%20rg-az104-lab-uksouth%20through%20the%20group%20sg-az104-lab-readers.png)

### Observation

The Reader role was inherited through the security group exactly as expected.

This was one of the most useful parts of the lab because it demonstrated how RBAC scales in larger environments.

No direct role assignment to the user was required.

---

## Step 5 - Review the Reader Role Definition

To understand exactly what access had been granted, I reviewed the role definition itself.

### Screenshot

![Reader Role Definition](./Azure%20Reader%20Role%20Definition%20and%20Permissions%20(JSON%20View).png)

### Key Finding

The Reader role is intentionally designed to provide visibility without allowing administrative changes.

```json
{
  "roleName": "Reader",
  "description": "View all resources, but does not allow you to make any changes."
}
```

Seeing the actual role definition helped connect the RBAC assignment with the permissions the user ultimately received.

---

## Step 6 - Test Permission Restrictions

To verify that the Reader role was working correctly, I signed in as the Reader user and attempted to create a new Resource Group.

### Screenshot

![Permission Denied](./Insufficient%20Permissions%20to%20Create%20Resource%20Groups%20in%20Azure%20Subscription.png)

### Result

Azure blocked the action and displayed a permissions error.

This confirmed that the Reader role was enforcing read-only access exactly as intended.

---

## Step 7 - Explore Additional RBAC Roles

While working through the lab, I decided to go a bit further and create another security group with the Virtual Machine Contributor role.

### Screenshot

![Additional Role Assignments](./Azure%20IAM%20-%20VM%20Contributor%20%26%20reader%20Role%20Assigned.png)

### Why I Added This

I wanted to better understand how different access levels could be assigned to different teams.

Example:

| Team | Role |
|--------|--------|
| Auditors | Reader |
| Infrastructure Team | Virtual Machine Contributor |

This helped me appreciate how Azure RBAC supports separation of duties and reduces unnecessary privilege.

---

## Step 8 - Remove the Reader Role Assignment

Once I had verified access, I removed the Reader role assignment from the security group.

### Screenshot

![Reader Role Removed](./Azure%20IAM%20-%20Reader%20Role%20Removed%20Successfully.png)

### Result

Azure confirmed that the Reader role assignment had been removed successfully.

---

## Step 9 - Verify Access Revocation

The final test was to confirm that removing the role assignment actually removed access.

### Screenshot

![Access Revoked](./Loss%20of%20Resource%20Group%20Visibility%20After%20Reader%20Role%20Removal.png)

### Result

After Azure RBAC changes propagated, the Resource Group was no longer visible to the Reader user.

This confirmed that access had been successfully revoked.

---

# ⚠ Challenges During the Lab

A few things initially caught me out:

- RBAC assignments were not applied instantly.
- Some permission changes required signing out and signing back into Azure.
- Access changes took several minutes to propagate.
- At first I thought I had misconfigured something, but the issue was simply propagation delay.

Although these were small issues, they helped me understand how Azure permissions behave in real-world environments.

---

# 💡 Key Takeaways

This lab gave me a much better understanding of Azure access management than simply reading documentation.

The biggest lessons I took away were:

- Assign roles to security groups instead of individual users.
- Follow the Principle of Least Privilege whenever possible.
- Always test both permission assignment and permission removal.
- RBAC inheritance makes access management easier to scale.
- Permission propagation delays are normal and should be expected.

Most importantly, I was able to see the full lifecycle of Azure access management from onboarding a user to revoking access.

---

# 🧠 Skills Practiced

### Azure

- Azure RBAC
- Azure IAM
- Resource Group Administration

### Microsoft Entra ID

- Guest User Management
- Security Group Administration
- Group-Based Authorization

### Security

- Principle of Least Privilege
- Role-Based Access Control
- Access Validation
- Access Revocation

### Cloud Administration

- Identity and Access Management (IAM)
- Permission Troubleshooting
- Azure Governance Fundamentals

---
