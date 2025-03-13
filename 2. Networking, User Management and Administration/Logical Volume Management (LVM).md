___ 

### **Logical Volume Manager (LVM) in Linux**

**LVM** (Logical Volume Manager) is a disk management system in Linux that allows for dynamic and flexible storage allocation. It abstracts storage management, making it easier to resize, extend, or shrink logical volumes (LVs) without disrupting the system.

___

### **Key Components of LVM**

1. **Physical Volume (PV):**
    
    - The base storage device or partition initialized for LVM.
    - Examples: Entire disk (`/dev/sdb`) or partition (`/dev/sda1`).
2. **Volume Group (VG):**
    
    - A storage pool created from one or more physical volumes.
    - Acts as a container from which logical volumes are allocated.
3. **Logical Volume (LV):**
    
    - The storage unit visible to the user and used by the file system.
    - LVs can be resized dynamically, provided there is free space in the volume group.

---

### **Steps for Creating LVM (PV: 500MB, VG: 500MB, LV: 300MB)**

#### **Step 1: Prepare a Partition**

1. Use `fdisk` to create a new partition:
    
    ```bash
    fdisk /dev/sdX
    ```
    
2. Set the partition type to LVM (`8e`).

#### **Step 2: Create a Physical Volume (PV)**

1. Initialize the partition as a PV:
    
    ```bash
    pvcreate /dev/sdX1
    ```
    
2. Verify the PV:
    
    ```bash
    pvdisplay
    ```
    

#### **Step 3: Create a Volume Group (VG)**

1. Create a VG named `vg_data` using the PV:
    
    ```bash
    vgcreate vg_data /dev/sdX1
    ```
    
2. Verify the VG:
    
    ```bash
    vgdisplay
    ```
    

#### **Step 4: Create a Logical Volume (LV)**

1. Create an LV named `lv_data` with 300MB size:
    
    ```bash
    lvcreate -L 300M -n lv_data vg_data
    ```
    
2. Verify the LV:
    
    ```bash
    lvdisplay
    ```
    

#### **Step 5: Format and Mount the LV**

1. Format the LV with a file system, e.g., `ext4`:
    
    ```bash
    mkfs.ext4 /dev/vg_data/lv_data
    ```
    
2. Create a mount point:
    
    ```bash
    mkdir /mnt/lvm_data
    ```
    
3. Mount the LV:
    
    ```bash
    mount /dev/vg_data/lv_data /mnt/lvm_data
    ```
    
4. Persist the mount by adding it to `/etc/fstab`:
    
    ```bash
    echo "/dev/vg_data/lv_data /mnt/lvm_data ext4 defaults 0 0" >> /etc/fstab
    ```
    

---

### **Advantages of LVM**

1. **Dynamic Resizing:** Resize LVs without unmounting (if the file system supports it).
2. **Flexibility:** Add or remove physical volumes without disrupting the system.
3. **Snapshots:** Take point-in-time snapshots for backups or testing.
4. **Improved Storage Utilization:** Combine multiple devices into a single logical storage pool.

---

### **Key Commands for LVM**

|Command|Description|
|---|---|
|`pvcreate`|Initialize a device as a physical volume.|
|`pvdisplay`|Display physical volume details.|
|`vgcreate`|Create a volume group.|
|`vgextend`|Add a physical volume to an existing VG.|
|`vgdisplay`|Display volume group details.|
|`lvcreate`|Create a logical volume.|
|`lvextend`|Extend the size of a logical volume.|
|`lvreduce`|Reduce the size of a logical volume.|
|`lvdisplay`|Display logical volume details.|
|`lvremove`|Remove a logical volume.|
|`lvscan`|Scan and display all logical volumes.|

---

### **LVM Practical Use Cases**

1. **Resizing Volumes:**
    
    - Extend VG:
        
        ```bash
        vgextend vg_data /dev/sdc
        ```
        
    - Extend LV and resize the file system:
        
        ```bash
        lvextend -L +100M /dev/vg_data/lv_data
        resize2fs /dev/vg_data/lv_data  # For ext2/3/4
        xfs_growfs /mnt/lvm_data        # For XFS  
        ```
        
2. **Creating Snapshots:**
    
    - Take a snapshot for backup or testing:
        
        ```bash
        lvcreate -L 100M -s -n lv_data_snapshot /dev/vg_data/lv_data
        ```
        
3. **Reducing LV Size (ext2/3/4 only):**
    
    1. Unmount the LV:
        
        ```bash
        umount /mnt/lvm_data
        ```
        
    2. Check and repair the file system:
        
        ```bash
        e2fsck -f /dev/vg_data/lv_data
        ```
        
    3. Resize the file system:
        
        ```bash
        resize2fs /dev/vg_data/lv_data 100M
        ```
        
    4. Reduce the LV:
        
        ```bash
        lvreduce -L 100M /dev/vg_data/lv_data
        ```
        
    5. Remount the LV:
        
        ```bash
        mount /mnt/lvm_data
        ```
        
___

### **Physical Extent (PE)**

**Physical Extent (PE)** is the smallest unit of storage within a physical volume (PV) in the Logical Volume Manager (LVM). When a physical volume is added to a volume group (VG), it is divided into chunks of equal size called physical extents.

### **Purpose of PE**

1. **Granularity:** PEs allow the allocation of storage at a finer level of detail, making it easier to utilize the available space efficiently.
2. **Dynamic Allocation:** Logical volumes (LVs) are created by grouping PEs, allowing flexible and dynamic allocation of storage.
3. **Storage Pooling:** By dividing the storage into PEs, LVM can create and manage logical volumes of varying sizes from the same pool of storage.

### **Advantages of PE**

1. **Efficient Space Utilization:**
    - Storage is allocated in chunks, ensuring no wastage of space within the physical volume.
2. **Scalability:**
    - Logical volumes can be resized easily by adding or removing PEs.
3. **Flexibility:**
    - Logical volumes are not restricted by the size or boundaries of a single physical disk, thanks to PEs.
4. **Consistent Management:**
    - Standardized PE sizes simplify the management and extension of physical volumes and volume groups.

### **Extension of PE**

1. **Adjusting PE Size:**
    
    - The size of a physical extent is set when the volume group is created and cannot be changed later. Default PE size is typically **4 MB**, but it can be customized.
    - Command to specify PE size during VG creation:
        
        ```bash
        vgcreate vg_name --physicalextentsize 32M /dev/sdX
        ```
        
2. **Extending Logical Volumes with PEs:**
    
    - Logical volumes can be extended by allocating additional PEs from the volume group:
        
        ```bash
        lvextend -l +<PE_count> /dev/vg_name/lv_name
        ```
        
3. **Verifying PE Distribution:**
    
    - View the allocation of PEs within a volume group using:
        
        ```bash
        vgdisplay
        ```
        

> **Note:** RHCSA question on **Physical Extent** is "Create LV partition, storage capacity 100 extents and each PE = 32M" 

---

### **Frequently Asked Questions**

#### **Can data loss occur when reducing LV size?**

Yes, if the reduction size is greater than the available storage on the LV. Always resize the file system before reducing the LV.

#### **What is the difference between ext2/3/4 and XFS in LVM?**

- **ext2/3/4**: Fully supports resizing, including reductions.
- **XFS**: Supports only extension, not reduction. To "shrink," you must back up data, reformat, and restore.

#### **Can XFS partitions be restructured to a new file system?**

Yes, you can remove the XFS partition and create a new one with a different file system, but this process erases all data. Always back up your data first.

---

### **Additional Commands for LVM**

- Display all available disks and partitions for LVM:
    
    ```bash
    lvmdiskscan
    ```
    
- Remove a VG:
    
    ```bash
    vgremove vg_data
    ```
    
- Remove a PV:
    
    ```bash
    pvremove /dev/sdb
    ```
	
- To display block devices and their mount points
	
	```bash
	lsblk
	```
	
- Most used command in LVM by System Admins
	```bash
	lvs -o +devices
	```
