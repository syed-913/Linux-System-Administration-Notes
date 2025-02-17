___

## **YUM** (Yellowdog Updater, Modified)

YUM is a package manager used in Linux distributions like CentOS and Red Hat to automatically download, install, update, and manage software packages. Unlike RPM (Red Hat Package Manager), YUM resolves dependencies automatically, making package management more efficient.

YUM operates by communicating with repository files located in the `/etc/yum.repos.d/` directory. These repository files contain links to remote package repositories where YUM fetches packages and their dependencies. YUM uses tools like `wget` or `curl` in the backend to download files, ensuring seamless automation.

YUM is widely used in enterprise environments for its automation, particularly for tasks like patching (updating packages), which is crucial for maintaining system security and functionality.

---

### **How YUM Works**

1. **Command Execution:** When a command like `yum install docker` is executed, YUM begins by searching the repository configuration files in `/etc/yum.repos.d/`.
2. **Locate Repository:** It uses these files to identify the remote repositories hosting the required package.
3. **Inventory File (`repomd.xml`):** YUM checks the repository's metadata (e.g., `repomd.xml`), which acts as an index of available packages.
4. **Dependency Resolution:** YUM identifies any dependencies required by the package and prepares to download them.
5. **Download:** It downloads the `.rpm` files for the package and its dependencies using `curl` or `wget`.
6. **Installation:** YUM installs the downloaded `.rpm` packages on the system, completing the process.

The **inventory file** (`repomd.xml`) acts as a _Table of Contents_ for the repository, guiding YUM to locate and download packages and their dependencies.

---

### **Advantages of YUM**

- **Dependency Management:** Automatically resolves and installs package dependencies.
- **Ease of Use:** Simplifies package installation and updates with minimal user intervention.
- **Automation:** Reduces manual effort compared to RPM, making it ideal for enterprise environments.

---

### **Troubleshooting YUM**

1. **Check Internet Connection:** Use `ping` to verify connectivity.
2. **Check Repositories:** Ensure repository files in `/etc/yum.repos.d/` are configured correctly and enabled.
3. **Clear YUM Cache:** Use `yum clean all` to resolve issues caused by corrupted metadata.
4. **Restart YUM Processes:** Kill and restart any stuck YUM processes.
5. **Verify DNS and Gateway:** Check DNS resolution and the default gateway to rule out network issues.

---

### **What is `EPEL`?**

**EPEL** (Extra Packages for Enterprise Linux) is a repository maintained by the Fedora Project. It provides additional packages for Enterprise Linux distributions, such as CentOS and Red Hat, which are not included in the default repositories. These packages often include newer or unique software that enhances functionality.

---

### **Additional Notes on Repositories**

There are numerous repositories available online, each containing unique packages. For example:

- **Base Repository:** Contains the essential packages for the Linux distribution.
- **EPEL:** Provides extra packages.
- **Third-Party Repositories:** Offer software not included in official repositories.

---
### **Local Repository**

A **Local Repository** is a directory on a hard disk that contains `.rpm` packages (along with their dependencies) used for package management without requiring internet access. It is particularly useful in offline environments or for maintaining control over the exact versions of packages being installed.

Unlike repositories hosted on the internet, a **Local Repository** is stored locally on a system or network, making it ideal for environments with limited or no internet connectivity.

---

### **Steps to Create a Local Repository**

1. **Mount the ISO File or Hard Disk:**
    
    - For VMs: Mount the ISO file containing the packages.
    - For physical storage: Mount the hard disk containing the `.rpm` files.
2. **Create a Partition for the Repository:**
    
    - Allocate a large partition (bigger than `/var/cache/PackageKit/metadata/9/baseos-9-x86_64/packages`) to store the repository.
    - Mount this partition to a new directory, e.g., `/local_repo`.
3. **Backup Existing Packages:**
    
    - Use the following command to copy the contents of the `/packages` directory to `/local_repo`:
        
        ```bash
        rsync -avp /path/to/source /local_repo
        ```
        
4. **Generate Metadata for the Repository:**
    
    - Create inventory files (similar to `repomd.xml`) for your local repository by running:
        
        ```bash
        createrepo_c /local_repo
        ```
        
5. **Configure YUM to Use the Local Repository:**
    
    - Create a repository configuration file in `/etc/yum.repos.d/`:
        
        ```bash
        [local]
        name=Local Repository
        baseurl=file:///path/to/local_repo
        enabled=1
        gpgcheck=0  # Disables GPG signature verification for offline repos.
        ```
        
6. **Test the Configuration:**
    
    - Run the following command to verify that the local repository is working:
        
        ```bash
        yum clean all
        yum repolist
        ```
        

---

### **Key Points:**

- A **Local Repository** ensures fast and secure package management without dependency on external networks.
- The `gpgcheck=0` option is typically disabled for local repositories since package verification is unnecessary in trusted environments.
- Always use tools like `rsync` for efficient copying to ensure all files, attributes, and permissions are intact.

___

## **Patching**

### **Checking for Updates**

- The first step is to check for available updates. This can be done using the command:
    
    ```bash
    yum check-update
    ```
    

### **Taking Approvals**

- Obtain approvals from all relevant departments, such as the Database team, Developers team, and others, before proceeding with updates.

### **Stopping Specific Services**

- Stop the service running on the server before updating it. If there is a **clustered environment**, clients should be transferred to an identical server to avoid downtime.

### **Data Backup**

- Always back up data before updating any package. Use tools like `rsync` to ensure the data is securely backed up:
    
    ```bash
    rsync -av /source_directory /backup_directory
    ```
    

### **Informing Teams**

- Inform all relevant teams that you are about to update packages to ensure coordination and awareness.

### **Updating Packages**

- It's uncommon to update all packages on the server at once (`yum update`), as some package versions might have specific compatibility requirements. Typically, only specific packages are updated:
    
    ```bash
    yum update <package_name_1> <package_name_2>
    ```
    
- To exclude certain packages while updating others:
    
    ```bash
    yum update --exclude=<package_name>
    ```
    

### **Handling Missing Packages**

- Sometimes, required packages might not be available in the default repositories listed in `/etc/yum.repos.d/`. In such cases, download the package from external repositories or configure additional repositories.

---

### **Question**

**If the kernel is updated while the server is in downtime and it fails to boot after the update, what should be done?**

### **Answer**

- Boot the server using the previous kernel version. This can usually be done from the boot menu (GRUB).
- Once the server is up, check logs to identify and resolve the issue:
    
    ```bash
    journalctl -k
    dmesg
    ```
    

---

## **Kernel Updating**

- Before updating the kernel, check its current version:
    
    ```bash
    uname -r
    ```
    
- To update the kernel:
    
    ```bash
    yum update kernel
    ```
    
- After updating, reboot the system to load the new kernel:
    
    ```bash
    reboot
    ```
    

---

## **YUM Roll-Back**

### **Purpose of Roll-Back**

- If an issue is reported after updating a package, such as incompatibility or malfunction, you may need to roll back the updated packages to their previous versions.

### **Steps for Roll-Back**

1. Check the history of installed packages:
    
    ```bash
    yum history
    ```
    
2. Roll back the system to a previous state:
    
    ```bash
    yum history rollback <transaction_ID>
    ```
    
3. Undo specific package updates if needed:
    
    ```bash
    yum history undo <transaction_ID>
    ```
    
