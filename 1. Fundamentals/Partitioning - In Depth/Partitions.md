# **Understanding Partitions in Linux**

Partitions play a critical role in efficient data management, system organization, and maintaining isolation between different data sets or operating systems.

---

## **What are Partitions?**

- A **partition** is a logically defined section of a hard drive, allowing for better organization and data management.
- **Benefits**:
    1. Simplifies backup and recovery by isolating data.
    2. Protects data in case of OS corruption (e.g., OS on `/` while user data is on `/home` remains safe).
    3. Facilitates installation of multiple operating systems.

For a detailed step-by-step guide on creating partitions, see [[Partition Creation Guide in Linux]].

---

## **Types of Partitions in Linux**

### **1. Primary Partitions**

- A disk can have up to **4 primary partitions**.
- These are directly accessible by the operating system.
- Bootable partitions (e.g., containing the OS) are typically primary partitions.

### **2. Extended Partition**

- Created to overcome the 4-primary-partition limit.
- Functions as a container for **logical partitions**.
- **Key Characteristics**:
    - Only **one extended partition** is allowed per disk.
    - Cannot store data directly.
    - Supports up to **63 logical partitions**.

### **3. Logical Partitions**

- Created within an extended partition.
- Can be used to store data, swap space, or directories like `/var` or `/home`.
- Identified by partition numbers starting from `sdx5`, with `sdx1` to `sdx4` reserved for primary or extended partitions.

---

## **Partition Measurement**

### **Cylinders, Heads, and Sectors**

- Older disks described partitions using cylinders, heads, and sectors (CHS).
- CHS is now replaced by **Logical Block Addressing (LBA)**, which simplifies the partitioning process on modern large disks.

#### **Key Terms**:

- **Sector**: The smallest unit of storage on a disk (typically 512 bytes or 4KB).
- **Cylinder**: A collection of tracks across all platters of a hard drive.

---

## **Partition Tables**

Partition tables store metadata about partitions and their sizes.

### **1. MS-DOS/MBR (Master Boot Record)**

- **Limits**:
    - Maximum of 4 primary partitions.
    - Supports disks up to **2TB** in size.
- **Disadvantages**:
    - Single-point failure: If the MBR is corrupted, the disk becomes unbootable.

### **2. GUID Partition Table (GPT)**

- **Features**:
    - Allows up to **128 primary partitions**.
    - Compatible with modern systems using UEFI.
    - Redundancy: Stores multiple copies of the partition table on the disk.
    - Supports disks larger than **2TB**.
- **Advantages**:
    - Improved reliability and flexibility.
    - Essential for systems with large storage capacities.

---

### **Links to Related Notes**

- [[Partition Creation Guide in Linux]]: A comprehensive step-by-step guide on creating partitions.
- [[Comprehensive Overview of Linux Filesystems, Mounting, and Repair Procedures]]: Learn more about managing partitions and file systems in Linux.

---

### **FAQ & Interview Questions**

1. **What’s the difference between a primary and a logical partition?**
    
    - Primary partitions are directly accessible, while logical partitions exist within an extended partition.
2. **Why is GPT preferred over MBR for modern systems?**
    
    - GPT supports larger disks, provides redundancy, and allows up to 128 partitions.
3. **What is the significance of `sdx5` in logical partitions?**
    
    - Logical partitions start numbering from `5`, as the first four slots are reserved for primary or extended partitions.
4. **How would you recover data from a corrupted MBR?**
    
    - Use tools like `testdisk` or `dd` to repair or back up the partition table.