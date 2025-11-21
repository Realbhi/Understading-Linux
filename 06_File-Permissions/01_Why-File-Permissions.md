### Permissions

Linux is inherently a multi-user operating system, allowing many users to access a shared server. For example, in an organization called ORG, developers, quality engineers (QEs), and DevOps engineers all need server access to deploy and manage applications.
**User Management** involves creating unique Linux users for each individual, such as dev1, dev2, qe1, and qe2.
Granting all users administrative or root privileges is unsafe and impractical.
Without file permissions, any user could access, modify, or delete files created by others, which can cause:
Corruption of the entire file system (e.g., deletion of critical directories like /sbin or /etc)
Loss of productivity if a user deletes or modifies scripts or files created by another user

**File permissions solve this problem by restricting access on a per-file and per-directory basis, ensuring users can only interact with files appropriately**

Key Points:
```
User management creates individual users and groups.
File permissions complement user management by controlling access to files and directories.
Linux sets default file permissions automatically on all files and folders.
Commands like chmod and chown allow administrators to modify these permissions as required.
```
