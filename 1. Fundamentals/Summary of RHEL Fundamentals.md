___

This document provides a summary of essential topics covered in the **RHEL - Fundamentals** module, highlighting key concepts, commands, and tools. Each topic links to detailed notes for further exploration.

---

### **1. File-System Hierarchy**

Defines the structure and role of directories under the root `/`, essential for Linux functionality.

- Detailed Note: [[File-System Hierarchy.canvas|File-System Hierarchy]]

---

### **2. Hard & Soft Links**

Explains the use cases, advantages, and distinctions between Hard Links and Symbolic (Soft) Links for data referencing and backup.

- Detailed Note: [[Hard & Soft Links|Hard & Soft Links]]

---

### **3. Ownerships and Permissions**

Covers Linux ownership and permissions to enforce security through access control, aligned with the CIA triad principles (Confidentiality, Integrity, Availability).

- Detailed Note: [[Ownerships and Permissions|Ownerships & Permissions]]

---

### **4. Run-Levels or Targets**

Explores operational states of Linux, transitioning from Run-Levels in SysV init to Targets in systemd, such as `graphical.target` and `multi-user.target`.

- Detailed Note: [[Targets or Run-Levels in Linux|Run-Levels or Targets]]

---

### **5. Partitions & `Fstab`**

Details the significance of partitions for data management and the role of the `/etc/fstab` file in automating partition mounting.

- Related Note: [[Partition Creation Guide in Linux]]
- Detailed Note: [[Partitions|Partitions & `Fstab`]]

---

### **6. Archive & Compression Tools**

Highlights tools and methods for file compression and archiving to aid backups and efficient data management.

- Key Tool: `rsync` for secure, incremental backups over SSH.
- Related Notes:
    - [[Backups and Their Differences in Linux]]
    - [[Rsync (A Backup Tool)]]

---

### **7. Filesystem Check & Repair**

Discusses tools like `e2fsck` for filesystem maintenance, addressing bad blocks to ensure system integrity and prevent data loss.

- Detailed Note: [[Comprehensive Overview of Linux Filesystems, Mounting, and Repair Procedures|File-System Check & Repair with `e2fsck`]]

---

### **8. Performance Monitoring**

Explains the significance of monitoring processes, daemons, and services using tools like `top`, `ps`, `sar`, and `lsof` for real-time system efficiency.

- Detailed Note: [[Processes, Daemons & SWAP - Performance Monitoring|Performance Monitoring]]

---
