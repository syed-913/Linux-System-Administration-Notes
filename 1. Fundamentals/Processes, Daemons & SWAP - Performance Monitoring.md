___
## **What are Processes?**

In Linux, a process is any active instance of a program. Each time a program runs, a new process is created with a unique identifier. Here are some commonly used commands for managing and gathering information on running processes:

```bash
ps -ef # Lists all running processes along with details.
```

```bash
lsof # Lists open files and their associated processes.
```

In Red Hat Enterprise Linux, the `/proc` directory contains files representing each process, which are **dynamically managed in RAM**.

To monitor processes in **real-time**, the `top` command is widely used:

```bash
top
```

We'll cover the `top` command in more detail later.

___
### **Role of RAM, Process IDs (PIDs), and Killing Processes**

**RAM** handles active processes. When a program is executed, it’s loaded from the hard disk into **RAM** for fast access and operation. Each process has a unique Process ID (**PID**). Some system processes, like `systemd`, maintain a consistent **PID** (e.g., PID 1) across sessions.

To close or terminate a process, use the following commands, ensuring you have the **PID** of the intended process:

```bash
kill 1234 # Replace "1234" with the target process PID.
pkill firefox # Terminates a process by its name.
```

> **Note**: Terminating a parent process will also end all of its child processes. For example, killing `systemd` would shut down all processes it manages.

To find the **PID** of a process by name:

```bash
pidof firefox # Finds the PID for a given process name.
pgrep fire # Searches for partial matches of process names.
```

To view parent-child relationships of processes graphically:

```bash
pstree # Displays processes in a tree structure.
```

--- 
## **Why is the `top` Command so Valuable?**

The `top` command is invaluable because it provides a **real-time view of system performance**, including CPU, memory, and process usage. It helps administrators **quickly identify resource bottlenecks** and troubleshoot performance issues, making it essential for **monitoring and managing system health**. Here are some other `top` variants:

```bash
htop # Adds color-coding and additional management features.
```

```bash
atop # Provides detailed system load views with CPU, memory, disk, network stats, and historical logging.
```

Other tools for specific monitoring tasks include:

```bash
iostat # Monitors CPU and disk I/O, useful for diagnosing storage-related issues.
netstat # Tracks network connections, routing tables, port listening, and usage stats.
vmstat # Shows performance data for memory, CPU, and I/O, ideal for spotting bottlenecks over time.
sar # Logs system performance metrics (CPU, memory, I/O) for trend analysis and diagnosing long-term issues.
pidstat # Displays the processes with their PID's in realtime like top but there are some differences.
mpstat # Shows the cpu utilization in more clean and extended way.
```

## **States of processes**

Following are the types of processes
### **1. Running (R)**

- A process in this state is actively executing instructions on the CPU.
- It can be either currently running or ready to run when CPU time is available.
- Processes in the "Running" state are scheduled by the operating system's CPU scheduler.
- Visible in process listing tools like `top` or `ps` as `R`.

### **2. Sleeping (S/D)**

#### **a. Interruptible Sleep (S)**

- The process is waiting for an event or signal to continue execution (e.g., I/O operation).
- It can be woken up by signals or events.
- Common in processes waiting for user input or disk I/O.
- Displayed as `S` in process listing tools.

#### **b. Uninterruptible Sleep (D)**

- The process is waiting for a hardware operation and cannot be interrupted by signals.
- Often caused by tasks such as reading/writing to a device.
- This state usually indicates processes stuck in kernel space.
- Displayed as `D` in process listing tools.

### **3. Stopped (T)**

- A process that has been paused, usually by a signal like `SIGSTOP` or for debugging purposes.
- The process remains in memory but does not execute until explicitly resumed.
- Useful during troubleshooting or debugging via tools like `gdb`.
- Displayed as `T` in process listing tools.

### **4. Zombie (Z)**

- A process that has completed execution but still has an entry in the process table.
- Occurs when the parent process hasn't read the exit status of the terminated child.
- Consumes no system resources except for the process table entry.
- Displayed as `Z` in process listing tools and should be cleaned by the parent process.

___
## **Buffer**
- Buffers are temporary storage areas in RAM used to hold data during I/O operations. They help manage the flow of data between processes or devices with different speeds.  
- Buffers can become overloaded due to memory management issues, such as high demand or inefficient resource allocation.  
- Memory leaks occur when an application fails to release allocated memory back to the system after it is no longer needed. This is typically caused by poorly written code in languages like C, C++, or other low-level programming languages.

## **Cache**
- Cache is a smaller, high-speed storage area that stores frequently accessed data to reduce latency and improve performance.  
- It is used to speed up operations by keeping data closer to the CPU or application, eliminating the need for repeated access to slower storage systems.  
- Caching can be implemented at various levels, such as CPU (L1, L2, L3 cache), disk (disk cache), or network (web caches like CDN).  
- Common examples of caching include browser caches for faster website loading and database caches to reduce query times.

___
## **What are Daemons in Linux?**

In Linux, **Daemons** are background processes, similar to services in Windows. Once started, a daemon runs continuously, handling periodic requests, often from **remote processes**. 

### Types of Daemons:

1. **Application Daemons**:
   - Impact limited to the application itself if stopped.
   - Examples: `crond`, `dhcpd`, `httpd`, `ftpd`.

2. **System Daemons**:
   - Stopping these can impact or crash the OS.
   - Examples: `systemd`, `nginx`, `init`, print spoolers.
___
# **SWAP**

### **What is SWAP?**

- **SWAP** is a dedicated space on a storage device (partition or file) that acts as virtual memory (or extended memory) for a Linux system. It supplements the physical RAM by providing additional memory for inactive processes when RAM is fully utilized.

---

### **On a Hardware Level, How SWAP Works**

- When the system’s physical RAM is fully utilized, the kernel moves less frequently used pages from RAM to the **SWAP** space on the hard disk. This frees up physical RAM for active processes.  
- For example, if a system with 128 MB of RAM runs out of memory due to multiple processes, a 256 MB SWAP partition can serve as overflow space. In this setup, active processes remain in RAM, while less active or idle ones are "swapped out" to the SWAP space.  
- While this enables the system to handle more processes, accessing data in SWAP is significantly slower than in RAM due to the speed difference between storage and RAM.

---

### **Why Is This Concept Called SWAP?**

- The term **SWAPPING** refers to the process of moving data between RAM and SWAP. Active processes run in RAM, while inactive ones are "swapped out" to the SWAP space to make room for more memory-intensive tasks.

---

### **Which File System Does SWAP Use?**

- **SWAP** doesn’t use a traditional file system. Instead, it uses a special format called **SWAP space**, managed by the kernel through a **Virtual File System (VFS)**. This format is optimized for paging and does not store files or directories like other file systems.

---

### **Why Is SWAP Required in a Linux File System?**

- **SWAP** serves several purposes:
  1. **Memory Overflow:** To handle memory demands when RAM is full.
  2. **System Stability:** Prevents the system from crashing due to out-of-memory (OOM) errors.
  3. **Hibernation:** Required for saving the state of the system to disk when it enters hibernation mode.
  4. **Performance Optimization:** Allows the system to free up RAM for active processes by moving idle ones to SWAP.

---

### **How to Build a SWAP Partition?**

1. **Create a SWAP Partition:**
   ```bash
   fdisk /dev/sdX
   ```
   - Create a new partition with type `82` (Linux SWAP).

2. **Format the SWAP Partition:**
   ```bash
   mkswap /dev/sdXn
   ```

3. **Activate the SWAP Partition:**
   ```bash
   swapon /dev/sdXn
   ```

4. **Add to `/etc/fstab` for Persistent Use:**
   ```plaintext
   /dev/sdXn none swap sw 0 0
   ```

---

### **When to Use SWAP?**

- SWAP is needed when the system's RAM is exhausted. However, its necessity depends on the following:
  - **RAM Size:** Systems with ample RAM (e.g., 16 GB or more) may rarely require SWAP.  
  - **Application Requirements:** Memory-intensive applications like databases or virtual machines might require SWAP regardless of RAM size.  
  - **System Purpose:** Systems used for hibernation require SWAP equal to or larger than the RAM size.

---

### **Does Windows Have a SWAP Equivalent?**

- Yes, Windows uses a **Paging File** as a virtual memory system.  
- However, the paging file is dynamically managed and not as efficient or predictable as Linux’s SWAP. It is often hidden from the user and doesn’t offer as much customization as Linux SWAP.

---

### **Is It Compulsory to Create SWAP of 2xRAM?**

- No, the 2xRAM rule is a guideline, not a strict requirement. The size of SWAP depends on:
  - System requirements (e.g., hibernation requires SWAP >= RAM).  
  - Applications running on the system.  
  - Disk space availability.  

>Note: Always refer to application and OS documentation for specific SWAP recommendations.

---

### **How to Know Which Process Is Using SWAP?**

- Check the `/proc` directory for detailed process information:
  1. Find the PID of the process (e.g., using `ps` or `top`).
  2. Navigate to the process's directory:
     ```bash
     cd /proc/<PID>
     ```
  3. Open the `status` file and look for the `VmSwap` field:
     ```bash
     grep VmSwap /proc/<PID>/status
     ```

---

### **Commands to Configure SWAP in Linux**

- **Check SWAP Usage:**
  ```bash
  swapon -s
  ```
  - Displays active SWAP partitions or files.

- **View All Partitions:**
  ```bash
  fdisk -l
  ```

- **Create SWAP Space in a File:**
  1. Create a SWAP file:
     ```bash
     dd if=/dev/zero of=/swapfile bs=1G count=2
     ```
  2. Set up the SWAP file:
     ```bash
     mkswap /swapfile
     ```
  3. Activate the SWAP file:
     ```bash
     swapon /swapfile
     ```
  4. Add it to `/etc/fstab` for persistence:
     ```plaintext
     /swapfile none swap sw 0 0
     ```

---

### **Additional Info**

- **Performance Impact of SWAP:**  
  Excessive use of SWAP (known as "swapping thrash") can degrade performance since disk I/O is slower than RAM.  
  Use tools like `vmstat` or `free -m` to monitor SWAP usage.

- **Tuning SWAP Behavior:**  
  Modify the `swappiness` value to control how aggressively the kernel uses SWAP:
  ```bash
  sysctl vm.swappiness=10
  ```
  - A lower value favors RAM over SWAP.

---
##### Queries:

**Q1:** What is a [`fork bomb`](# ":(){:|:&};:") in Linux?

A fork bomb is a denial-of-service (DoS) attack where a small piece of code continuously replicates processes, depleting system resources. When it maxes out the server's resources, it causes the system to crash. (To prevent) Setting process limits across a system using the /etc/security/limits.conf file. This is the preferred method since the setting can be applied across all profiles, thus mitigating the risk in editing each user’s .profile settings.

**Q2:** Why can’t the process with **PID 1 (systemd)** be killed?

**Answer**: `systemd` (PID 1) is the **init process**, the first process started by the kernel, which manages all other processes. Killing it will force the system to shut down or restart, as it’s critical to system stability.