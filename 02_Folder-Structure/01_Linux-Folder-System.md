#  Understanding Linux File-System Structure
---

<img width="1460" height="610" alt="image" src="https://github.com/user-attachments/assets/b7abf47a-a9d7-460a-b4cf-3ad3af526b08" />



All files and directories in Linux stem from a single root directory `/`.

Think of `/` as the *starting point* (like “C:\” in Windows). Every file, folder, and device is accessible from this root hierarchy.

###  Hierarchy Overview

```
/
├── bin/       → Essential user binaries
├── sbin/      → System binaries for administrators
├── lib/       → Shared libraries for programs
├── usr/       → User programs and system resources
├── etc/       → System configuration files
├── var/       → Variable data like logs and caches
├── home/      → Home directories for regular users
├── root/      → Home directory for root user
├── opt/       → Optional third-party software
├── tmp/       → Temporary files (auto-cleared)
├── dev/       → Device files
├── proc/      → Virtual filesystem (process info)
├── sys/       → Virtual filesystem (hardware info)
├── run/       → Runtime data for processes
├── srv/       → Data for services (e.g., web servers)
├── mnt/       → Temporary mount point
├── media/     → Mount point for removable drives
└── boot/      → Bootloader and kernel files (unused in containers)
```

---

## ** Explanation of Key Directories**

###  **/bin → /usr/bin**

Contains essential user commands needed for basic operation by all users.
Examples: `ls`, `cp`, `mv`, `cat`, `date`, `grep`.

###  **/sbin → /usr/sbin**

Holds system binaries and administrative tools, typically used by the root user.
Examples: `useradd`, `mount`, `ifconfig`, `reboot`.

> In modern Linux, `/bin`, `/sbin`, and `/lib` are **symbolic links** to `/usr/bin`, `/usr/sbin`, and `/usr/lib` to centralize system binaries.

###  **/lib → /usr/lib**

Houses shared libraries and kernel modules needed for programs in `/bin` and `/sbin`.
Equivalent to `.dll` files in Windows.

###  **/usr (User System Resources)**

Contains most of the system’s user-space software:

* `/usr/bin` – Non-essential user commands.
* `/usr/sbin` – Additional admin commands.
* `/usr/lib` – Libraries for the above binaries.
* `/usr/share` – Shared data, icons, docs, etc.
* `/usr/games` – Game binaries.

###  **/boot**

Contains critical files for booting the OS — kernel images and bootloader configs (e.g., GRUB).

> In Docker containers, this is usually **empty**, since containers don’t manage boot processes.

###  **/etc**

Stores **system-wide configuration files**:

* `/etc/passwd` → user accounts
* `/etc/hosts` → hostname-to-IP mappings
* `/etc/os-release` → OS info
  Changing files here directly affects system behavior.

###  **/home**

Contains user directories and personal data.

* Example: `/home/ubuntu`, `/home/abishek`
  Each user’s files, settings, and scripts live here.

###  **/root**

The **home directory for the root user**.
It’s an exception — it lives directly under `/` rather than inside `/home`.

###  **/opt**

For installing **third-party or custom software** not managed by the system package manager (e.g., manually installed tools like Java or VSCode).

###  **/var**

Holds **variable data** that changes frequently:

* Logs → `/var/log`
* Caches → `/var/cache`
* Package data → `/var/lib`
* Spools (e.g., mail, print jobs)

###  **/tmp**

Used for **temporary files** created by processes or users.
Cleared on reboot — suitable for transient data.

###  **/proc**

A **virtual filesystem** that provides system and process information dynamically.

* Example: `/proc/cpuinfo`, `/proc/meminfo`, `/proc/[pid]/`

###  **/sys**

Another virtual filesystem exposing information about hardware and the kernel.

* Example: `/sys/class/net/` (network interfaces)

###  **/run**

Stores runtime information (e.g., process IDs, sockets).
Contents exist only while the system is running.

###  **/dev**

Contains **device files**, which represent hardware components.

* Example: `/dev/sda` (disk), `/dev/null`, `/dev/tty`.

###  **/srv**

Used by service daemons (like Apache or FTP) to store data they serve to clients.
Often empty in containers.

###  **/mnt**

A temporary **mount point** for external filesystems (e.g., mounting a USB drive manually).

###  **/media**

Automatic mount point for removable media (e.g., USB drives, CD-ROMs).

###  **/data**

Custom mount point (in your case from Docker).
Example: `/data` may map to a host directory on your Mac via Docker bind mount.

---

## ** Symbolic Links Between Core Directories**

To simplify system management, modern Linux distributions link traditional folders to their counterparts under `/usr`:

| Link    | Target        | Purpose          |
| ------- | ------------- | ---------------- |
| `/bin`  | → `/usr/bin`  | User binaries    |
| `/sbin` | → `/usr/sbin` | System binaries  |
| `/lib`  | → `/usr/lib`  | Shared libraries |

This design ensures consistency and avoids duplicate directories across root and `/usr`.

---

## ** Executing Commands and the PATH Variable**

When you type a command like `ls`, the system needs to know **where** to find the corresponding binary file.

###  PATH Environment Variable

`PATH` is a list of directories that the shell searches when you enter a command.

View it using:

```bash
echo $PATH
```

Typical output:

```
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

###  How it works

1. You type `ls`.
2. The shell looks in each directory listed in `PATH`, in order.
3. When it finds `/bin/ls`, it executes it.
4. If not found in any listed path → `command not found`.

You can temporarily add directories to PATH:

```bash
export PATH=$PATH:/home/ubuntu/scripts
```

This allows you to run executables in `/home/ubuntu/scripts` without typing their full path.

---

## ** Summary Table**

| Category                | Directory                                | Description                                    |
| ----------------------- | ---------------------------------------- | ---------------------------------------------- |
| **Essential Binaries**  | `/bin`, `/sbin`, `/usr/bin`, `/usr/sbin` | Core command binaries                          |
| **Libraries**           | `/lib`, `/usr/lib`                       | Shared libraries and modules                   |
| **Configuration**       | `/etc`                                   | System configuration files                     |
| **User Data**           | `/home`, `/root`                         | User-specific data and root’s home             |
| **Third-party Apps**    | `/opt`                                   | Optional or custom software                    |
| **Logs and Cache**      | `/var`                                   | Variable data, logs, caches                    |
| **Temporary Storage**   | `/tmp`, `/run`                           | Ephemeral files and runtime data               |
| **Virtual Filesystems** | `/proc`, `/sys`                          | Kernel, process, and hardware info             |
| **Mount Points**        | `/mnt`, `/media`, `/data`                | For external or host-mounted storage           |
| **Boot Files**          | `/boot`                                  | Bootloader and kernel (not used in containers) |
| **Service Data**        | `/srv`                                   | Data for network services                      |

---

✅ **In essence:**

* `/` is the root of everything.
* `PATH` defines where the shell looks for commands.
* Each directory has a distinct system role — from binaries to configs to user files.
* Docker containers follow the same structure but omit boot-related components.


