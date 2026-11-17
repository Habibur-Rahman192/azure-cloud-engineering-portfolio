# Implementing Azure RBAC with Microsoft Entra ID
## Project Summary
This project demonstrates the implementation of Azure Role-Based Access Control (RBAC) using Microsoft Entra ID security groups. The objective was to provide controlled, read-only access to Azure resources while enforcing the Principle of Least Privilege.
---
## Business Requirement
Provide secure access to Azure resources without granting modification privileges to users.
---
## Solution
- Created a guest user in Microsoft Entra ID.
- Created a dedicated security group for Reader access.
- Assigned the built-in **Reader** role at the Resource Group scope.
- Added the guest user to the security group.
- Validated inherited permissions.
- Removed the Reader role assignment and verified access revocation.
---
## Technologies Used
- Microsoft Azure
- Microsoft Entra ID
- Azure RBAC
- Azure Resource Groups
- Azure IAM
---
## Implementation Evidence
### Guest User Provisioning
![Guest User Added](./Guest%20user%20has%20been%20added%20to%20the%20tenant.png)
A guest user was created and onboarded into Microsoft Entra ID for RBAC testing.
---
### Reader Role Assignment
![Reader Role Assignment](./Azure%20RBAC%20Reader%20Role%20Assignment%20for%20Lab%20Users.png)
The security group was assigned the built-in Reader role at the Resource Group scope.
---
### Validation of Read-Only Access
![Reader Access Validation](./lab-reader-01%20has%20a%20Reader%20role%20assignment%20on%20the%20Azure%20resource%20group.png)
The user successfully inherited Reader permissions and could view resources without the ability to modify them.
---
### Permission Restriction Verification
![Permission Denied](./Insufficient%20Permissions%20to%20Create%20Resource%20Groups%20in%20Azure.png)
An attempt to create a Resource Group failed as expected, confirming that write permissions were not granted.
---
### Reader Role Definition
![Reader Role Definition](./Azure%20Reader%20Role%20Definition%20and%20Permissions%20(JSON%20View).png)
Review of the Azure Reader role confirmed that permissions were limited to read-only actions.
---
### Reader Role Removal
![Reader Role Removed](./Azure%20IAM%20-%20Reader%20Role%20Removed%20Successfully.png)
The Reader role assignment was removed from the security group.
---
### Access Revocation Validation
![Access Revoked](./Loss%20of%20Resource%20Group%20Visibility%20After%20Reader%20Role%20Removal.png)
After role removal, the user could no longer view the Resource Group, confirming successful access revocation.
---
## Results
| Validation Test | Outcome |
|----------------|---------|
| User can sign in to Azure | ✅ Pass |
| User can view resources | ✅ Pass |
| User can create resources | ❌ Blocked |
| User can modify resources | ❌ Blocked |
| User can delete resources | ❌ Blocked |
| Access removed after role removal | ✅ Verified |
---
## Key Skills Demonstrated
- Microsoft Entra ID
- Azure Role-cess Management (IAM)
- Security Group Management
- Azure Resource Group Administration
- Least Privilege Access Control
- Access Review and Validation
---
## Key Takeaways
This project demonstrates how Azure RBAC can be used to securely manage access through Microsoft Entra ID security groups. The implementation successfully enforced read-only access, prevented unauthorized administrative actions, and validated access removal when permissions were revoked.
---
**Author:** Habibur Rahman  


