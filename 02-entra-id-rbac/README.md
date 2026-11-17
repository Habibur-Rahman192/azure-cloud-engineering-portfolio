# Azure Entra ID RBAC: Implementing and Validating Reader Access

## Overview

This project demonstrates the implementation of **Azure Role-Based Access Control (RBAC)** using **Microsoft Entra ID**. The objective was to grant read-only access to Azure resources, validate permission restrictions, and verify access revocation after removing the role assignment.

---

## Technologies Used

- Microsoft Azure
- Microsoft Entra ID
- Azure RBAC
- Azure Resource Groups
- Azure IAM

---

## Architecture

```text
Azure Subscription
│
└── Resource Group (rg-az104-lab-uksouth)
    │
    └── Reader Role Assignment
         │
         └── Security Group (sg-az104-lab-readers)
              │
              └── Guest User (lab-reader-01)
```

---

## Step 1: Add Guest User to Microsoft Entra ID

A guest user was invited and added to the Microsoft Entra ID tenant for testing Azure RBAC permissions.

![Guest User Added](./Guest%20user%20has%20been%20added%20to%20the%20tenant.png)

*Figure 1: Guest user successfully added to the tenant.*

---

## Step 2: Assign Reader Role

The security group **sg-az104-lab-readers** was assigned the built-in **Reader** role at the Resource Group scope.

![Reader Role Assignment](./Azure%20RBAC%20Reader%20Role%20Assignment%20for%20Lab%20Users.png)

*Figure 2: Reader role assignment configured for the lab user group.*

---

## Step 3: Validate Reader Access

The guest user successfully signed in and accessed the Resource Group.

### Allowed Actions

✅ View Resource Groups  
✅ View resources and configurations  
✅ View monitoring information  
✅ Browse Azure portal resources  

### Restricted Actions

❌ Create resources  
❌ Modify resources  
❌ Delete resources  
❌ Manage access permissions  

![Reader Access](./lab-reader-01%20has%20a%20Reader%20role%20assignment%20on%20the%20Azure%20resource%20group.png)

*Figure 3: Reader user accessing Azure resources with read-only permissions.*

---

## Step 4: Attempt Resource Group Creation

To validate RBAC restrictions, the Reader user attempted to create a new Resource Group.

Azure denied the request because the Reader role does not include write permissions.

![Permission Denied](./Insufficient%20Permissions%20to%20Create%20Resource%20Groups%20in%20Azure.png)

*Figure 4: Permission error displayed when attempting to create a Resource Group.*

### Validation

```text
You do not have permissions to create resource groups under this subscription.
```

This confirms that the Reader role provides visibility into resources while preventing modifications.

---

## Step 5: Review Reader Role Definition

The built-in Reader role definition was reviewed through Azure IAM.

![Reader Role Definition](./Azure%20Reader%20Role%20Definition%20and%20Permissions%20%28JSON%20View%29.png)

*Figure 5: Azure Reader role definition displayed in JSON format.*

### Key Permission

```json
"actions": [
  "*/read"
]
```

### Reader Role*Cap*bilities

| Permission | Access |
*------------|--------|
| View Reso*rces | ✅ |
|*View Configurations | ✅ |
| Monito* Resources | ✅ |
| Create Resource* | ❌ |
|*Modify Resources | ❌ |
| Delete Re*ources | ❌ |
| Assign Permissions * ❌ |

*--

## Step 6: Remove Reader Role *ssignment

The Reader role assignm*nt was removed from the security*group.

![Reader Role Removed](./A*ure%20IAM%20-%20Reader%20Role*20Removed%20Successfully.png)

*Fi*ure 6: Reader role assignment succ*ssfully removed.*

---

*# Step 7: Verify Access Revocation*
After the role assignment was rem*ved, the user signed in again and *o longer had*visibility of the Resource Group.
*![Access Revoked](./Loss%20of%20Re*ource%*0Group%20Visibility%20After%20*eader%20Role%20Removal.png)

**igure 7: Access revoked after Read*r role removal.*

---

## Results
*| Test | Result |
|--------|------*-|
| Sign in to Azure Portal | ✅ |*| View Resource Group | ✅ |
| Crea*e Resources | ❌ |
| Modify Resourc*s | ❌ |
| Delete Resources | ❌ |
|*Manage Permissions | ❌ |
| Access *fter Role Removal | ❌ |

---

## S*ills Demonstrated*
- Microsoft Entra ID
- Azure RB*C
- Identity and Access Management*(IAM)
* Security Groups
- Resource*Group Administration*- Least Privilege Access Control
-*Azure Governance

---

## Key Take*ways

- Azure RBAC enables granula* permission management.
- Security*groups simplify*role administration.
- The Reader *ole provides visibility without mo*ification rights.
- Access*is immediately*revoked after role removal.
- RBAC*supports the*Principle of Least Privilege.

---*
## Author

**Habibur Rahman**

Az*re Cloud Engineering Portfolio*
Git*ub: https://github.com/Habibur-R*hman192/azure-cloud-engineering-*ortfolio