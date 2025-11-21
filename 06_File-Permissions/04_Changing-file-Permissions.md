### The chmod command allows changing file permissions either through symbolic (alphabetic) or numeric (octal) modes.
---

**Symbolic Mode Syntax:**
```
u = user
g = group
o = others
```
```
Permissions: r (read), w (write), x (execute)
Operators: = (set), + (add), - (remove)
```

**Examples:**
```
chmod u=rwx,g=rw,o=r hello_world.sh — owner gets all permissions, group gets read/write, others get read only.
```
---

### Numeric Mode Syntax:

---

**Each permission assigned a numeric value:**
```
Read = 4
Write = 2
Execute = 1
```

**Sum the values for each set (user, group, others)**

Examples:
```
chmod 755 hello_world.sh means:
User: 7 (4+2+1 = rwx)
Group: 5 (4+0+1 = r-x)
Others: 5 (4+0+1 = r-x)
```
```
chmod 644 hello_world.sh means:
User: read/write (6)
Group: read-only (4)
Others: read-only (4)
```
---

**Practical Demonstration:***
- Removing read permission from others on hello_world.sh using chmod o-r hello_world.sh caused QE to lose read access.
- Adding execute permission for the user to run the script with chmod u+x hello_world.sh.
- Granting full permissions to others with chmod o=rwx hello_world.sh allowed QE to execute and modify the script.
