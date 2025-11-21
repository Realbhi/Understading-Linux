### Permissions on directories work similarly but with important distinctions:
---

**Execute (x)** permission on a directory allows a user to enter or traverse the directory.
***Read (R)** permission allows listing contents of the directory.
***Write (w)*** permission allows creating, deleting, or renaming files inside the directory.

**Example:**

The home directory of the developer user has permissions set to 750:
```
User: read, write, execute (7)
Group: read and execute (5)
Others: no permissions (0)
```
The QE user, as others, cannot enter or list the developer’s directory (Permission denied).

### Hierarchical Permission Model:
---

- Access to files depends on permissions of the parent directory.
- Using the analogy of a bank and a locker:
 - You need permission to enter the bank (directory) first.
 - Then you need permission to open the locker (file) inside the bank.
 - Even if a file has open permissions, lack of permission on its containing directory prevents access.


**Demonstration:**

Changing /tmp directory permissions to 700 (only owner can access).
Even if files within /tmp have open permissions (777), users without /tmp directory access cannot access those files.
