
Partitions enable the logical separation of data and efficient management of disk storage. This guide provides a step-by-step process for creating and configuring a partition.

---

<h2><center><b> How to Make a Partition in Linux </b></center></h2>

### **1. Identify the Disk**

Before creating a partition, determine the available disks and their existing partitions.

```bash
lsblk        # Displays block devices in a tree structure
fdisk -l     # Lists all partitions with details like size and type
```

---

### **2. Run `fdisk` to Partition the Disk**

Use the `fdisk` utility to create, delete, or modify partitions.

```bash
sudo fdisk /dev/sdX  # Replace 'X' with the appropriate disk identifier (e.g., /dev/sda)
```

Steps inside `fdisk`:

1. Press **`n`** to create a new partition.
2. Choose **`p`** for a primary partition or **`e`** for an extended partition.
3. Assign a partition number (e.g., 1, 2, etc.).
4. Specify starting and ending sectors or press Enter to accept defaults.
5. Define the partition size (e.g., `+2G` for 2GB).
6. Press **`w`** to write the changes to disk.

---

### **3. Refresh the Kernel’s Partition Table**

To apply the changes without rebooting, update the kernel's partition table:

```bash
sudo partx -a /dev/sdX    # Adds all new partitions to the kernel
```

Or use:

```bash
sudo partprobe /dev/sdX   # Notifies the kernel about partition table updates
```

---

### **4. Format the Partition**

Select a filesystem type and format the partition:

```bash
sudo mkfs.ext4 /dev/sdX1  # Replace X1 with the partition identifier
```

Alternative filesystems:

- **XFS**: `sudo mkfs.xfs /dev/sdX1`
- **FAT32**: `sudo mkfs.vfat /dev/sdX1`

---

### **5. Create a Mount Point and Mount the Partition**

Define a directory to mount the partition:

```bash
sudo mkdir /mnt/my_partition       # Create a mount point
sudo mount /dev/sdX1 /mnt/my_partition  # Mount the partition
```

Verify the mount:

```bash
df -h  # Lists mounted filesystems
```

---

### **6. Update `/etc/fstab` for Automatic Mounting**

To mount the partition automatically on boot:

1. Retrieve the partition's UUID:
    
    ```bash
    sudo blkid /dev/sdX1
    ```
    
2. Add the UUID to `/etc/fstab`:
    
    ```
    UUID=your-partition-uuid /mnt/my_partition ext4 defaults 0 0
    ```
    

---

### **7. Unmount the Partition**

If needed, unmount the partition:

```bash
sudo umount /mnt/my_partition
```

---

### **Command Explanations**

- **`fdisk`**: Tool for creating, deleting, and managing disk partitions.
- **`partx`/`partprobe`**: Updates the kernel with new partition information.
- **`mkfs`**: Formats the partition with a specified filesystem.
- **`mount`/`umount`**: Attaches or detaches a partition to/from the filesystem.
- **`lsblk`**: Lists all block devices.
- **`blkid`**: Displays partition UUIDs and other metadata.

---

### **FAQ & Interview Questions**

1. **Why would you use `partprobe` instead of rebooting?**
    
    - To avoid downtime while updating the kernel about partition changes.
2. **What’s the difference between `partx` and `partprobe`?**
    
    - Both notify the kernel about partition changes, but `partx` allows finer control, such as adding specific partitions.
3. **How do you decide the filesystem type (e.g., ext4 vs. xfs)?**
    
    - Choose **ext4** for general-purpose use and compatibility. Opt for **XFS** for systems requiring fast throughput for large files.
4. **What happens if `/etc/fstab` is misconfigured?**
    
    - The system may fail to boot. Use recovery mode or a live environment to correct `/etc/fstab`.