___
### **Run-Levels**

In Linux, Run-Levels define different operational states of the system, determining which services and processes are running. For example, if someone forgets their **root** password, it can be recovered using rescue mode. Traditional systems using SysV init have numbered Run-Levels from 1 to 6. The `init` command switches between Run-Levels, while `runlevel` checks the previous and current Run-Level. There are 7 Run-Levels in Red Hat 6.

#### **Run-Level Details:**

1. **Run-Level 0: Halt**
   - **Purpose:** Shuts down the system.
   - **Usage:** To safely power off the machine.
   - **Details:** Terminates all processes and powers down the system.

2. **Run-Level 1: Single-User mode**
   - **Purpose:** For maintenance or troubleshooting.
   - **Usage:** Used when system recovery or repairs are needed.
   - **Details:** Only the **root** user can log in; minimal services are started.

3. **Run-Level 2: Multi-User mode without Networking**
   - **Purpose:** Provides a multi-user environment without network services.
   - **Usage:** In environments where networking isn't required.
   - **Details:** Allows multiple users but no network services like NFS.

4. **Run-Level 3: Multi-User mode with Networking**
   - **Purpose:** Standard full multi-user mode with network support.
   - **Usage:** Common for servers.
   - **Details:** Starts all services for a fully functional system.

5. **Run-Level 4: Undefined/Custom**
   - **Purpose:** Customizable; not used by default.
   - **Usage:** For user-defined purposes like special applications.
   - **Details:** Allows custom scripts or services.

6. **Run-Level 5: Multi-User mode with GUI**
   - **Purpose:** Multi-user mode with graphical interface.
   - **Usage:** Standard for desktop systems.
   - **Details:** Includes all services of Run-Level 3 plus a GUI.

7. **Run-Level 6: Reboot**
   - **Purpose:** Reboots the system.
   - **Usage:** To restart the system safely.
   - **Details:** Terminates all processes and restarts the system.

---

### **Systemd**

**Systemd** is a modern system and service manager for Linux, designed to replace the older SysV init system. It introduces parallel service startup, on-demand daemon activation, and is more efficient than SysV. Instead of Run-Levels, systemd uses **targets** to define system states.

#### **Functionalities and Differences:**

| **Feature**               | **Systemd**                                          | **SysV (System V init)**                           |
| ------------------------- | ---------------------------------------------------- | -------------------------------------------------- |
| **Initialization**        | Parallel initialization of services.                 | Sequential initialization of services.             |
| **Speed**                 | Faster boot times due to parallelism.                | Slower boot times due to sequential start.         |
| **Dependency Management** | Automatically handles service dependencies.          | Limited; relies on manual scripting.               |
| **Configuration Files**   | Uses declarative unit files (`.service`, `.target`). | Uses shell scripts (`/etc/rc.d` or `/etc/init.d`). |
| **Service Monitoring**    | Built-in monitoring and auto-restart on failure.     | Limited; needs additional scripts.                 |
| **Socket Activation**     | Supports socket-based service activation.            | No socket-based activation.                        |
| **Logging**               | Integrated with `journald`.                          | Logging handled by tools like `syslog`.            |
| **Runlevels/Targets**     | Uses targets for system states.                      | Uses numbered Run-Levels (0-6).                    |
| **Resource Control**      | Manages resources with cgroups.                      | No built-in resource management.                   |
| **Modularity**            | Highly modular with various unit types.              | Less modular, more monolithic.                     |
| **On-Demand Services**    | Services start on-demand via sockets.                | Must be manually or boot-started.                  |
| **System State**          | Targets like `multi-user.target`.                    | Run-Levels like 3 (multi-user).                    |
| **Error Handling**        | Auto-restarts services on failure.                   | Requires manual recovery.                          |
| **Customization**         | Easily customizable with unit files.                 | Customization via shell scripts.                   |

---

### **Targets in Systemd**

Targets group units (services, sockets, mounts, etc.) and replace Run-Levels. Each target represents a specific system state and manages which services should be running.

#### **Common Systemd Targets:**

1. **default.target**
   - **Purpose:** Default system state after boot.
   - **Equivalent to:** Run-Level 3 or 5, depending on setup.
   - **Customization:** Change with:
     ```bash
     sudo systemctl set-default multi-user.target
     ```

2. **poweroff.target**
   - **Purpose:** Shuts down the system.
   - **Equivalent to:** Run-Level 0.

3. **rescue.target**
   - **Purpose:** Single-user mode for recovery.
   - **Equivalent to:** Run-Level 1.
   - **Services Started:** Minimal set for troubleshooting.

4. **multi-user.target**
   - **Purpose:** Multi-user mode without GUI.
   - **Equivalent to:** Run-Level 3.
   - **Services Started:** Full multi-user, including network.

5. **graphical.target**
   - **Purpose:** Multi-user mode with GUI.
   - **Equivalent to:** Run-Level 5.
   - **Services Started:** All services of `multi-user.target` plus GUI.

6. **reboot.target**
   - **Purpose:** Reboots the system.
   - **Equivalent to:** Run-Level 6.

7. **emergency.target**
   - **Purpose:** Minimal recovery mode with a shell.
   - **Details:** Similar to `rescue.target` but fewer services start; only root filesystem is mounted.

---

### **Unit Files in Systemd**

Units are systemd's building blocks, defining services, devices, sockets, mounts, and targets.

- **Types of Units:**
  - **Service units** (`.service`): Define services.
  - **Target units** (`.target`): Group other units.
  - **Socket units** (`.socket`): For IPC.
  - **Timer units** (`.timer`): Schedule service execution.
  - **Device units** (`.device`): Represent devices.
  - **Mount units** (`.mount`): Manage filesystem mounts.
  - **Automount units** (`.automount`): Automount filesystems.
  - **Swap units** (`.swap`): Manage swap spaces.

#### **Dependency and Ordering:**
- **Wants/Requires:** Define dependencies.
- **Before/After:** Specify activation order.

---

### **Managing Targets Commands:**

1. **List Targets**:
   ```bash
   systemctl list-units --type=target
   ```

2. **Switch Targets**:
   ```bash
   sudo systemctl isolate graphical.target
   ```

3. **Set Default Target**:
   ```bash
   sudo systemctl set-default multi-user.target
   ```

4. **View Default Target**:
   ```bash
   systemctl get-default
   ```

---

# How 'Traditional Logs' are configured in `Systemd` system

In **SysVinit**, logs like `auth.log`, `syslog`, and others are stored as plain text files in the `/var/log/` directory. However, with **systemd**, logs are managed by **journald** and stored in binary format, accessed via `journalctl`. The logs traditionally found in `/var/log/` are now integrated into the systemd journal.

Here's how the traditional SysVinit logs map to the journalctl scope in a systemd-managed system:

---

### **Log Mapping: SysVinit vs. systemd**

|**Traditional Log File (SysVinit)**|**journalctl Scope (systemd)**|
|---|---|
|`/var/log/auth.log`|`journalctl _SYSTEMD_UNIT=sshd.service` (or `journalctl -u sshd`)|
|`/var/log/syslog`|General logs: `journalctl` (equivalent to `/var/log/messages`)|
|`/var/log/messages`|General logs: `journalctl`|
|`/var/log/kern.log`|Kernel logs: `journalctl _TRANSPORT=kernel`|
|`/var/log/daemon.log`|Daemon logs: `journalctl _SYSTEMD_UNIT=<service>.service`|
|`/var/log/cron`|Cron logs: `journalctl _COMM=cron`|
|`/var/log/dmesg`|Boot logs: `dmesg` (or `journalctl -k`)|
|`/var/log/mail.log`|Mail logs: `journalctl _SYSTEMD_UNIT=postfix.service`|
|`/var/log/boot.log`|Boot logs: `journalctl -b`|

---

### **Key Points to Note:**

1. **Centralized Logging**:
    
    - `journald` consolidates all logs (system logs, authentication, daemons, etc.) into a single journal. You use `journalctl` to filter and view them.
2. **Filtering with `journalctl`**:
    
    - Use `journalctl` with specific filters to narrow down logs by service, priority, or boot session.
    - Examples:
        - View SSH authentication logs: `journalctl _COMM=sshd`
        - View kernel logs: `journalctl -k`
        - View logs for the current boot: `journalctl -b`
3. **Compatibility**:
    
    - On systemd systems, traditional `/var/log/` files might still exist if `rsyslog` or `syslog-ng` is installed and configured. These services can write logs to text files in addition to the systemd journal.

---

### **What to Do If You Miss Traditional Logs:**

- **Enable `rsyslog` or `syslog-ng`**:
    
    - These tools can be used alongside `systemd` to maintain traditional log files in `/var/log/`.
    - Install and enable `rsyslog`:
        
        ```bash
        sudo yum install rsyslog
        sudo systemctl enable rsyslog
        sudo systemctl start rsyslog
        ```
        
    - After enabling, logs like `/var/log/auth.log` and `/var/log/syslog` will reappear.
- **Export logs from `journalctl`**:
    
    - You can manually export logs if needed:
        
        ```bash
        journalctl > /path/to/syslog
        ```
        

---

### **Conclusion**:

Logs like `auth.log`, `syslog`, and others from SysVinit are not directly stored as text files in systemd systems. Instead, they are centralized in the **journal**, which you can query using `journalctl`. If you need traditional log files for compatibility or habit, enable `rsyslog` or similar services.

