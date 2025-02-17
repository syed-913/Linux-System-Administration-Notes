___
### **1. Basics of Booting and Hard Disk Functionality**

- **RAM vs. Hard Disk in Booting**:  
    While RAM provides temporary storage, it lacks the ability to boot an operating system. The hard disk holds the OS and loads it into RAM during the boot process.
    
- **Command Execution and Storage**:  
    Commands often operate on data stored in RAM. However, critical operations affecting the OS or filesystems (e.g., `rm -rf /*`) ultimately access the hard disk through RAM.
    

---

### **2. [[File-Systems & Their Differences|Overview of File Systems]]**

Linux offers diverse file systems optimized for various needs:

- **ext2**: Simple, no journaling (prone to data loss in crashes).
- **ext3**: Journaling for metadata and data integrity.
- **ext4**: Advanced journaling, large file support, and efficient directory management.
- **XFS, Btrfs, NTFS**: For specialized use cases like high-performance servers or cross-platform compatibility.

---

### **3. Persistent Mounting with `/etc/fstab`**

- **Purpose of Mounting**:  
    Mounting links a directory to a partition, but these associations reside in volatile RAM and vanish after reboot.
    
- **Making Mounts Persistent**:  
    The `/etc/fstab` file stores mount configurations, ensuring directories and partitions automatically remount on system startup. Key fields include:
    
    1. Device or UUID.
    2. Mount point.
    3. File system type.
    4. Permissions/Options.
    5. Dump frequency.
    6. Boot-time check order.
- **Testing Changes**:  
    After editing `/etc/fstab`, apply changes without rebooting using:
    
    ```bash
    mount -a
    ```
    

---

### **4. Using FSCK for Filesystem Checks and Repairs**

- **Purpose of FSCK**:  
    FSCK (File System Check) scans and repairs file systems for bad blocks and logical corruption.
    
- **Key Commands**:
    
    - **Identify Bad Blocks**:
        
        ```bash
        badblocks -v /dev/sda1
        ```
        
    - **Repair Filesystem**:
        
        ```bash
        fsck -y /dev/sda1
        ```
        
    - **Advanced Repair**:
        
        ```bash
        e2fsck -yc /dev/sda1
        ```
        
- **Steps for Repair**:
    
    1. Identify bad blocks with `badblocks`.
    2. Unmount the affected partition:
        
        ```bash
        umount /dev/sda1
        ```
        
    3. Repair with `fsck` or `e2fsck`.

---

### **5. Causes of Bad Blocks**

1. **Unclean Shutdowns**: Filesystem inconsistencies due to abrupt power-offs.
2. **Power Fluctuations**: Voltage irregularities affecting disk sectors.
3. **Faulty Applications**: Poorly coded software causing filesystem errors.

---

### **6. In-Depth `/etc/fstab` Configuration**

- **Field 6 (Boot-Time Checks)**:
    
    - **0**: Skip checking the partition during boot.
    - **1**: Check this partition first (e.g., root).
    - **2**: Subsequent partitions to check.
- **Using UUIDs**:
    
    - Unique identifiers ensure accurate device identification.
    - Find UUID with:
        
        ```bash
        blkid /dev/sda1
        ```
        
- **Best Practices**:
    
    - Avoid live editing `/etc/fstab` on production servers.
    - Use proper syntax to prevent boot failures.

---

### **7. Permissions and Options in `/etc/fstab`**

The 4th field in `/etc/fstab` configures mount permissions and options:

|**Option**|**Description**|
|---|---|
|**`rw/ro`**|Mount as read-write (`rw`) or read-only (`ro`).|
|**`suid/nosuid`**|Enable/disable privilege escalation through `setuid`/`setgid`.|
|**`exec/noexec`**|Allow (`exec`) or prevent (`noexec`) execution of binaries.|
|**`noatime`**|Disable access time updates for better performance.|
|**`relatime`**|Update access time only if the modification time is older, balancing performance and tracking needs.|
|**`noauto`**|Prevent automatic mounting at boot. Useful for removable devices.|

---

### **8. Handling Errors in `/etc/fstab`**

- **Symptoms**:  
    Incorrect entries in `/etc/fstab` can lead to the system booting into **Maintenance Mode**.
    
- **Solution**:
    
    - Edit `/etc/fstab` to fix errors (e.g., incorrect UUIDs).
    - Exit Maintenance Mode: Press `Ctrl+D`.

---

### **Links to Related Notes**

- [[File-System Hierarchy.canvas|File-System Hierarchy]]: Overview of Linux directory organization.
- [[Hard & Soft Links]]: Understanding links and their role in data management.

---

### **FAQ & Interview-Style Questions**

1. **What happens if `/etc/fstab` is misconfigured?**  
    The system may fail to boot or enter Maintenance Mode. Correct the file and reboot.
    
2. **Why use UUIDs in `/etc/fstab`?**  
    Device names (e.g., `/dev/sda1`) can change. UUIDs ensure consistent identification.
    
3. **How do you safely edit `/etc/fstab`?**  
    Test changes with `mount -a` before rebooting.
    
4. **What is the purpose of `fsck -f`?**  
    Forces a check even if the filesystem appears clean.