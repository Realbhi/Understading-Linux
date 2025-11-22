### Real-World Scenario
---

- A typical Linux server may start with a set volume size (e.g., 8 GB in AWS EC2).
- Over time, log files and installed software consume disk space.
- Administrators may need to add storage volumes, format, and mount them to extend capacity.

---

**Key Concepts**

**Block Storage:**  Raw storage provided by cloud providers (e.g., AWS Elastic Block Storage - EBS).

**Partitions:**     Volumes are split into partitions; each can be formatted and mounted separately.

**File Systems:**   Common formats include ext4 and XFS; formatting converts block storage to usable file systems.

**Mounting:**       Attaching a volume or partition to a directory path to make it accessible.

---

**Note :: Whatever volume is added cant be access or used by the application directly . we need to partition the volume , need to format into ext4 or XFS , then mount to /mnt .**
More about it in next section ....
