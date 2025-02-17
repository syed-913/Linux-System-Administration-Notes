___

## **Kernel Overview**

The kernel acts as an interpreter, translating commands from the Command Line Interface (CLI) into machine-readable code (binary). This enables hardware-level understanding and execution. It can be compared to the engine of a car, controlling all drivers and processes without storing them.

- Multiple kernels can coexist in the same OS, but only one runs at a time.
- Kernel updates create new kernel binaries rather than overwriting existing ones.
- Key files:
    - `config`: Acts as the kernel manual.
    - `initramfs`: A compressed archive preparing the system for boot.
- Kernel binary files are stored in `/boot` (e.g., `vmlinuz-5.14.0-533.el9.x86_64`).
- Kernel drivers, or modules, are stored in `/lib/modules/<kernel_version>/kernel/` with the `.ko` extension (Kernel Object).

---

## **Driver Installation and Management**

### **Driver Installation**

Drivers are often installed from source code or `.rpm` packages. To make them persistent:

- Use `modprobe` or `insmod` to load `.ko` files into **RAM**.
- Add an entry in `/etc/modprobe.d/dist.conf` to ensure persistence.

### **Driver Management Commands**

- **List drivers:**
    
    ```bash
    find . -iname "*.ko"
    ```
    
- **Loaded drivers in RAM:**
    
    ```bash
    lsmod
    ```
    
- **Add a driver to RAM:**
    
    ```bash
    modprobe <driver_name>
    ```
    
- **Remove a driver from RAM:**
    
    ```bash
    rmmod <driver_name>
    ```
    
- **Persistent removal of drivers:**
    
    ```bash
    rm -rf /path/to/kernel/driver.ko
    ```
    
- **Module info:**
    
    ```bash
    modinfo <driver_name>
    ```
    
- **Blacklist a driver:**  
    Add to `/usr/lib/modprobe.d/dist-blacklist.conf`:
    
    ```bash
    blacklist <driver_name>
    ```
    

---

## **Kernel Tuning**

Kernel tuning involves modifying parameters to optimize performance. Changes are applied temporarily through `/proc/sys` and made persistent in `/etc/sysctl.conf`.

### **Key Tuning Scenarios**

#### **1. Increasing File Descriptors**

Applications may fail to load into **RAM** due to kernel file limits. Adjust the file descriptor limit:

- Check current processes:
    
    ```bash
    lsof | wc -l
    ```
    
- Temporarily modify the limit:
    
    ```bash
    echo "<desired_value>" > /proc/sys/fs/file-max
    ```
    
- Persistent change:
    
    ```bash
    echo "fs.file-max=<desired_value>" >> /etc/sysctl.conf
    ```
    

#### **2. Adjusting Swapiness**

Optimize **SWAP** performance by tuning `/proc/sys/vm/swapiness`.

- Check current value:
    
    ```bash
    cat /proc/sys/vm/swapiness
    sysctl -a | grep -i "swapiness"
    ```
    
- Temporarily adjust:
    
    ```bash
    echo "<desired_value>" > /proc/sys/vm/swapiness
    sysctl -w vm.swapiness=<desired_value>
    ```
    
- Persistent change:
    
    ```bash
    echo "vm.swapiness=<desired_value>" >> /etc/sysctl.conf
    ```
    

---

## **Types of Kernels**

### **1. Monolithic Kernel**

Includes all drivers and services in a single large process, offering high performance but limited modularity.

### **2. Micro Kernel**

Minimizes functionality to essential processes, with additional services running in user space for better stability and modularity.

### **3. Hybrid Kernel**

Combines features of Monolithic and Micro kernels, balancing performance and modularity.

### **4. Exo Kernel**

Focuses on direct hardware access by applications, offering high performance but requiring advanced application design.

### **5. Nano Kernel**

A minimalist kernel providing only basic functionality, delegating most operations to higher-level systems.

---
