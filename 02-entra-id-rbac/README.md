Microsoft Entra ID and Azure RBAC: Implementing and Validating Reader Access
Project overview
This lab demonstrates how to use Microsoft Entra ID security groups and Azure Role-Based Access Control (RBAC) to provide controlled, read-only access to an Azure resource group.
The lab follows a practical access-management workflow:
Add a guest account to the Microsoft Entra ID tenant.
Add the guest account to a security group.
Assign the built-in Reader role to that group at resource-group scope.
Sign in as the test user and validate the access granted.
Attempt an operation that requires write permissions and confirm that it is denied.
Review the Reader role definition.
Remove the role assignment and verify that access is no longer available.
The main security principle demonstrated is least privilege: users should receive only the permissions required for their tasks.
Skills demonstrated
Microsoft Entra ID guest-user management
Security-group creation and membership
Azure RBAC role assignments
Role-assignment scope
Built-in Azure role definitions
Read-only access validation
Permission troubleshooting
Access revocation
Least-privilege access control
Technologies used
Microsoft Azure portal
Microsoft Entra ID
Azure Role-Based Access Control (RBAC)
Azure Resource Groups
Lab environment
Item               Value
Resource group     rg-az104-lab-uksouth Security group     sg-az104-lab-readers Test guest user    lab-reader-01 Azure role         Reader Assignment scope   Resource group
Security note: The screenshots are included as practical evidence. Do not publish passwords, access tokens, tenant secrets, subscription-sensitive information, or personal data in screenshots.
Architecture
Microsoft Entra ID tenant
└── Security group: sg-az104-lab-readers
    └── Guest user: lab-reader-01

Azure subscription
└── Resource group: rg-az104-lab-uksouth
    └── RBAC assignment: Reader
        └── Assigned to: sg-az104-lab-readers
The guest user receives the permissions through group membership. The role is assigned to the group rather than directly to the individual user.
Implementation and validation
1. Add a guest user to Microsoft Entra ID
A guest account named lab-reader-01 was added to the Microsoft Entra ID tenant for access testing.
�
Evidence 1: Guest user added to the Microsoft Entra ID tenant.
2. Assign the guest user to a security group
The security group sg-az104-lab-readers was used to manage access. The test guest user was added as a member of this group.
Using a group makes access administration easier to maintain than assigning permissions individually to multiple users.
3. Assign the Reader role at resource-group scope
The built-in Reader role was assigned to sg-az104-lab-readers at the rg-az104-lab-uksouth resource-group scope.
�
Evidence 2: Reader role assignment configured for the lab group.
The assignment at resource-group scope limits the role's intended scope to that resource group and its supported child resources. It does not grant subscription-wide Reader access.
4. Validate the guest user's access
The test user was used to check whether the role assignment provided the expected read-only access.
�
Evidence 3: The test user receives Reader access through the security group.
The Reader role is intended to allow viewing resources and settings without granting permission to make changes.
5. Test an operation requiring write permissions
The test user attempted to create a resource group. Azure denied the operation because the Reader role does not include the required write permission.
�
Evidence 4: Resource-group creation was denied for the Reader user.
This test demonstrates the difference between being able to view Azure resources and being authorised to create or modify them.
6. Review the Reader role definition
The built-in Reader role definition was inspected in Azure to understand the permissions behind the assignment.
�
Evidence 5: Reader role definition inspected in JSON view.
The role's management-plane actions include the wildcard read action:
{
  "actions": [
    "*/read"
  ]
}
This is a simplified extract of the role definition, not the complete JSON document. The role definition also contains other fields that determine its full behaviour.
7. Remove the Reader role assignment
The Reader role assignment was removed from the security group at the resource-group scope.
�
Evidence 6: Reader role assignment removed.
8. Verify access after role removal
After the role assignment was removed, the test user was checked again. The resource group was no longer visible to that user in the tested portal view.
�
Evidence 7: Resource-group visibility was lost after the role assignment was removed.
Azure role-assignment changes can take time to propagate. In a production environment, allow for propagation and sign-in/session effects before concluding that an access change has failed.
Results
Test                    Expected outcome        Observed result
Add guest user to       Guest account is        Completed tenant                  present
Add user to security    User is a group member  Completed group
Assign Reader role to   Read access at          Completed group                   resource-group scope
View the resource group Allowed                 Verified
Create a resource group Denied                  Verified as Reader
Review Reader role      Read permissions can be Completed definition              inspected
Remove Reader role      Group no longer has     Completed assignment              that assignment
Check access after      Resource group is no    Verified removal                 longer visible in the
tested view
Key learning outcomes
Group-based access: Assigning roles to security groups simplifies ongoing access administration.
Scope matters: Assigning Reader at resource-group scope is more limited than assigning it at subscription scope.
Least privilege: Reader is suitable when a user needs visibility but should not be able to make changes.
Validate permissions: Testing both allowed and denied actions helps confirm that access behaves as intended.
Understand role definitions: Reviewing the role definition helps explain why an operation is permitted or denied.
Revoke access carefully: Removing a role assignment is part of the access lifecycle; verify the result and account for propagation.
Additional evidence
The repository also contains an image named Azure IAM - VM Contributor & reader Role Assigned.png. It is retained as additional role-assignment evidence. Its exact scope and purpose should be documented separately if it represents a different exercise.
Conclusion
This lab provided practical experience with Microsoft Entra ID guest users, security groups and Azure RBAC. It demonstrated how to grant read-only access at a specific resource-group scope, test the resulting permissions, inspect the role definition and remove access when it was no longer required.
These are core identity and access-management tasks for an Azure administrator and support the AZ-104 learning objectives.
Portfolio: Azure Cloud Engineering Portfolio