\# Azure Entra ID RBAC: Implementing and Validating Reader Access



\## Overview



This project demonstrates the implementation and validation of \*\*Azure Role-Based Access Control (RBAC)\*\* using \*\*Microsoft Entra ID\*\*. The lab focuses on assigning the built-in \*\*Reader\*\* role to a user, validating read-only access to Azure resources, and confirming access restrictions after the role assignment is removed.



The objective is to understand how Azure RBAC enforces the \*\*Principle of Least Privilege\*\* by granting users only the permissions required to perform their tasks.



\---



\## Objectives



\- Create and configure an Azure Resource Group.

\- Create a Security Group for Reader access.

\- Add a guest user to the Microsoft Entra ID tenant.

\- Assign the Reader role at the Resource Group scope.

\- Validate read-only access to Azure resources.

\- Demonstrate restrictions when attempting administrative actions.

\- Remove the Reader role assignment.

\- Verify access removal after RBAC permission revocation.



\---



\## Technologies Used



\- Microsoft Azure

\- Microsoft Entra ID

\- Azure Role-Based Access Control (RBAC)

\- Azure Resource Manager (ARM)

\- Azure Resource Groups

\- Azure IAM (Identity and Access Management)



\---



\## Lab Architecture



```text

Azure Subscription

│

└── Resource Group (rg-az104-lab-uksouth)

&#x20;   │

&#x20;   └── Reader Role Assignment

&#x20;         │

&#x20;         └── Security Group (sg-az104-lab-readers)

&#x20;                   │

&#x20;                   └── Guest User (lab-reader-01)

```



\---



\## Step 1: Add Guest User to Microsoft Entra ID



A guest user was invited and added to the Microsoft Entra ID tenant to test Azure RBAC permissions.



\### Outcome



\- Guest user successfully added to the tenant.

\- User accepted the invitation.

\- User successfully signed in to the Azure portal.



\### Screenshot



!\[Guest User Added](./Guest%20user%20has%20been%20added%20to%20the%20tenant.png)



\*\*Figure 1:\*\* Guest user successfully added to the Microsoft Entra ID tenant.



\---



\## Step 2: Assign Reader Role to the Resource Group



The security group \*\*sg-az104-lab-readers\*\* was assigned the built-in \*\*Reader\*\* role at the scope of the Resource Group \*\*rg-az104-lab-uksouth\*\*.



\### Outcome



\- Permissions assigned through group membership.

\- User inherited Reader access automatically.

\- Read-only access granted to Azure resources.



\### Screenshot



!\[Reader Assignment](./Azure%20RBAC%20Reader%20Role%20Assignment%20for%20Lab%20Users.png)



\*\*Figure 2:\*\* Reader role assignment configured for the lab user group.



\---



\## Step 3: Verify Reader Access



The user signed in using the guest account and successfully accessed the Azure Resource Group.



\### Allowed Actions



✅ View Resource Groups



✅ View resources and configurations



✅ View monitoring information



✅ Browse Azure portal resources



\### Restricted Actions



❌ Create resources



❌ Modify resources



❌ Delete resources



❌ Manage access permissions



\### Screenshot



!\[Reader Access Validation](./lab-reader-01%20has%20a%20Reader%20role%20assignment%20on%20the%20Azure%20resource%20group.png)



\*\*Figure 3:\*\* Reader user successfully accessing Azure resources with read-only permissions.



\---



\## Step 4: Attempt Resource Group Creation



To validate RBAC restrictions, the Reader user attempted to create a new Resource Group.



Azure denied the operation because the Reader role does not include write permissions.



\### Screenshot



!\[Permission Denied](./Insufficient%20Permissions%20to%20Create%20Resource%20Groups%20in%20Azure.png)



\*\*Figure 4:\*\* Permission error displayed when attempting to create a Resource Group.



\### Error Message



```text

You do not have permissions to create resource groups under this subscription.

```



\### Validation



This confirms that the Reader role provides visibility into resources while preventing any modifications.



\---



\## Step 5: Review Reader Role Definition



The built-in Reader role definition was reviewed through Azure IAM.



\### Screenshot



!\[Reader Role Definition](./Azure%20Reader%20Role%20Definition%20and%20Permissions%20(JSON%20View).png)



\*\*Figure 5:\*\* Azure Reader role definition displayed in JSON format.



\### Key Permission



```json

"actions": \[

&#x20; "\*/read"

]

```



\### Reader\*Role Capabilities



| Permission | \*ccess\*|

|------------|---------\*

| View Resources | ✅ |

\* View Configurations | ✅ |

| Monit\*r\*Resources | ✅ |

| Create Resources\*| ❌ |

| Modify Resources | ❌ |

| D\*lete Resources | ❌ |

| Assign\*Permissions | ❌ |



\---



\## Step 6\* Remove Reader Role Assignment



Th\* Reader role assignment\*was removed from the security grou\*.



\### Outcome



\- Access permissio\*s revoked successfully.

\- Azure RB\*C changes propagated.

\- User no lo\*ger inherited Reader permissions.

\*### Screenshot



!\[Reader Role Remo\*ed](./Azure%20IAM%20-%20Reader%20R\*le%20Removed\*20Successfully.png)



\*\*Figure 6:\*\*\*Reader role assignment successfull\* removed.



\---



\## Step 7: Verify \*ccess Removal



After the Reader ro\*e was removed, the user signed int\* Azure again.



The Resource Group \*as no longer visible, confirming t\*at access had been revoked.



\### S\*reenshot



!\[Access Revoked](./Loss\*20of%20Resource%20Group%20\*isibility%20After%20\*eader%20Role%20Removal.png)



\*\*Fig\*re 7:\*\* Azure\*portal after Reader role removal s\*owing loss of Resource\*Group visibility.



\### Validation \*esults



| Test Case | Result |

|--\*---------|---------|

| Sign in to \*zure Portal | ✅ Pass\*|

| View Resource Group | ❌ Fail |\*| View Azure Resources | ❌ Fail |

\* Create Resources | ❌ Fail |

| Mod\*fy Resources\*| ❌ Fail |

| Delete Resources | ❌ \*ail |



\---



\## Security Principles\*Demonstrated



\### Principle of Lea\*t Privilege



Users should receive \*nly the permissions required to pe\*form their responsibilities.



\### \*ole-Based Access Control (RBAC)



A\*cess to Azure resources is control\*ed through role assignments at spe\*ific scopes, reducing the\*need for direct permission\*assignments.



\### Separation of Du\*ies



The Reader role allows monito\*ing and auditing activities withou\* granting the ability to make chan\*es to production resources.



\---



\*# Key Learning Outcomes



\- Impleme\*ted Azure RBAC using Microsoft Ent\*a ID.

\- Assigned permissions\*through Security Groups\*

\- Applied the built-in Reader rol\*\*at Resource Group scope.

\- Validat\*d inherited access through group m\*mbership.

\- Demonstrated read-only\*access restrictions.

\- Verified Az\*re authorization failures for unau\*horized operations.

\- Examined Rea\*er role permissions in JSON format\*

\- Successfully removed role assig\*ments.

\- Confirmed loss of access \*ollowing permission revocation.



\-\*-



\## Conclusion



This lab success\*ully demonstrated the implementati\*n and management of Azure RBAC usi\*g Microsoft Entra ID. The Reader r\*le provided sufficient permissions\*to view Azure resources while prev\*nting all modification actions. Af\*er the Reader role assignment was \*emoved, access to the Resource Gro\*p was immediately lost, confirming\*the effectiveness of Azure RBAC in\*enforcing least-privilege security\*practices.



This exercise highligh\*s how organizations can securely m\*nage access to Azure resources by \*ssigning permissions through group\* and RBAC roles rather than granti\*g excessive privileges directly to\*users.



\---



\*# Author



\*\*Habibur Rahman\*\*



\*loud Engineering Portfolio



\### Sk\*lls Demonstrated



\- Microsoft Entr\* ID

\- Azure RBAC

\- Identity\*and Access Management \*IAM)

\- Resource Group Security\*- Role Assignments

\- Least Privile\*e Access Control\*

