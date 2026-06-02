# Create roles - Microsoft Fabric | Microsoft Learn
Source: https://learn.microsoft.com/en-us/fabric/onelake/security/create-manage-roles
Captured: 2026-05-30 | Action: reference-only

## Summary
Microsoft Fabric's OneLake security roles control access to tables and folders within Fabric data items. Roles can be created, edited, or deleted with immediate effect, requiring Fabric Write/Reshare permissions. Membership is assigned via direct user/group addition or virtual members based on item permissions.

## Key Points
- Roles require alphanumeric names starting with a letter, with Read permission mandatory for all roles.
- Scope options: 'All data' (applies to future folders) or 'Selected data' (explicit table/folder selection).
- Two membership methods: direct (manual addition) or virtual (based on Fabric item permissions like ReadAll/Write).
- Editing data access or members updates instantly; virtual members cannot be manually removed.
