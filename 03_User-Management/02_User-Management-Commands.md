#  **User Management in Linux**

Linux is a multi-user system — every person or service runs under a user account.
User management involves creating, modifying, and deleting users, as well as setting passwords and permissions.

---

## **1. Creating Users with `useradd`**

### **Command:**

```bash
useradd "username"
```

### **Explanation:**

Creates a new user account entry in the system.

But by **default**:

*  **Does NOT** ask for a password.
*  **Does NOT** create a home directory.
*  **Does NOT** prompt for user details (like full name or phone).
*  Creates a record in `/etc/passwd` and reserves a UID (user ID).

**Result:**

* The user exists but can’t log in until a password is assigned.
* No personal files or shell settings are created yet.

Check:

```bash
cat /etc/passwd | grep username
```

You’ll see a line like:

```
username:x:1001:1001::/home/username:/bin/sh
```

---

### **Command:**

```bash
useradd -m username
```

### **Explanation:**

* Creates a **home directory** automatically at `/home/username`.
* Copies default config files from `/etc/skel/` (like `.bashrc`, `.profile`).
* Still does **not** set a password — you must assign one manually with `passwd`.

---

### **Command:**

```bash
useradd -s /bin/bash username
```

### **Explanation:**

Specifies the user’s **default login shell**.

* By default, users get `/bin/sh`.
* Using `-s /bin/bash` gives them the full Bash shell environment.

You can combine flags:

```bash
useradd -m -s /bin/bash username
```

Creates the home directory and sets `/bin/bash` as the shell.

---

## **2. Viewing User Account Files**

### **Command:**

```bash
cat /etc/passwd
```

### **Explanation:**

Shows **user account information** for all users on the system.

Each line = 1 user, formatted as:

```
username:x:UID:GID:comment:home_directory:login_shell
```

Example:

```
ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
```

| Field          | Description                                             |
| -------------- | ------------------------------------------------------- |
| username       | Login name                                              |
| x              | Placeholder indicating password stored in `/etc/shadow` |
| UID            | User ID (numeric identifier)                            |
| GID            | Group ID (primary group)                                |
| comment        | Full name or info field                                 |
| home_directory | Default user home folder                                |
| login_shell    | Shell launched at login                                 |

---

### **Command:**

```bash
cat /etc/shadow
```

### **Explanation:**

Stores **encrypted passwords** and password aging info for each user.
Only the **root** user can view this file.

Format:

```
username:encrypted_password:last_change:min:max:warn:inactive:expire:reserved
```

Example:

```
ubuntu:$6$sdsfHk3sd$abcxyz...:19130:0:99999:7:::
```

> The second field contains the hashed password (not readable directly).

---

## **3. The Friendlier `adduser` Command**

### **Command:**

```bash
adduser "username"
```

### **Explanation:**

* `adduser` is a **higher-level wrapper** around `useradd`.
* It’s **interactive** — it:

  * Asks for a password.
  * Creates the home directory automatically.
  * Prompts for user details (full name, room number, etc.).
* It’s the **recommended** way to add users on Debian/Ubuntu systems.

---

## **4. Switching Users**

### **Command:**

```bash
su - "username"
```

### **Explanation:**

* **Switch User** — opens a shell as another user.
* The `-` loads that user’s full environment (like login shell and PATH).
* You’ll be prompted for the target user’s password (unless you’re root).

Example:

```bash
su - ubuntu
```

Switches you to user *ubuntu* as if you logged in directly.

---

## **5. Managing Passwords and Policies**

### **Set a password:**

```bash
passwd "username"
```

* Prompts to set or change a user’s password.
* The password is stored (hashed) in `/etc/shadow`.

---

### **Set password expiration policy:**

```bash
chage -M 90 username
```

* Forces the user to change their password every 90 days.
* Managed by the **chage (change age)** command.

Check user password policy:

```bash
chage -l username
```

---

### **Lock a user account:**

```bash
passwd -l username
```

* Locks the account by adding an exclamation mark `!` before the encrypted password in `/etc/shadow`.
* Prevents login (useful for temporary disabling).

---

### **Unlock a user account:**

```bash
passwd -u username
```

* Removes the `!` and restores the account’s access.

---

## **6. Modifying Existing Users with `usermod`**

The `usermod` command lets you **edit existing user properties**.

---

### **Change username:**

```bash
usermod -l new_username old_username
```

* Renames a user account (login name).
* Doesn’t automatically rename their home directory (unless you specify `-d`).

---

### **Change home directory:**

```bash
usermod -d /new/home/directory -m username
```

* `-d` → sets a new home directory path.
* `-m` → moves the existing contents from the old home to the new one.

---

### **Change default shell:**

```bash
usermod -s /bin/zsh username
```

* Updates the user’s login shell to `/bin/zsh`.

---

## **7. Deleting Users**

### **Remove user (keep home directory):**

```bash
userdel username
```

* Deletes the user account entry but **keeps** their home directory and files.

---

### **Remove user and delete home directory:**

```bash
userdel -r username
```

* Deletes the user account **and** their home directory `/home/username`.
* Also removes their mail spool and personal data.

---

##  Key Files Involved

| File              | Description                           |
| ----------------- | ------------------------------------- |
| `/etc/passwd`     | User account details                  |
| `/etc/shadow`     | Encrypted passwords and expiry info   |
| `/etc/group`      | User group information                |
| `/etc/skel/`      | Default files copied to new home dirs |
| `/home/username/` | User’s personal directory             |

