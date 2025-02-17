___
# Packages in Linux

Packages are software bundles distributed as a single file, containing multiple files and instructions about where they should be placed on the system. In Windows, `.exe` files are commonly used to execute and install software, writing data to specific directories or partitions.  

Similarly, Linux-based operating systems handle software installation using **package managers**, but they do not natively support `.exe` files. Instead, each Linux distribution has its own package format and manager. For this guide, we will focus on Red Hat-based systems.

---

## Red Hat Package Manager (RPM)

The **Red Hat Package Manager (RPM)** is a package management system used in Red Hat-based distributions like Fedora, CentOS, and RHEL (Red Hat Enterprise Linux). It uses `.rpm` files as its package format. RPM allows users to install, query, update, and remove software packages efficiently.

### Querying Packages
```bash
rpm -qa
```
- **List all installed packages.**

```bash
rpm -qla zsh-5.8-9.el9.x86_64.rpm
```
- **List all files included in a specific package.**

```bash
rpm -qi zsh-5.8-9.el9.x86_64.rpm
```
- **Display detailed information about a package.**

---
### Installing, Removing, and Upgrading Packages
```bash
rpm -ivh zsh-5.8-9.el9.x86_64.rpm
```
- **Install a package with a progress indicator (`-i`) and a hash bar (`-vh`).**

```bash
rpm -e zsh-5.8-9.el9.x86_64.rpm
```
- **Erase (remove) a package along with its files.**

```bash
rpm -Uvh zsh-5.8-9.el9.x86_64.rpm
```
- **Upgrade an existing package or install it if it's not already present.**

---
## Downloading Files in Linux

To download software or scripts from the internet, two commonly used commands are `curl` and `wget`.

### Examples:
```bash
curl -O https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh
```
- **Download a file from the URL using `curl`. The `-O` option saves the file with its original name.**

```bash
wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh
```
- **Download the file using `wget`.**

---

### Additional Information

- **Why RPM?**  
RPM provides a standardized way to manage software on Red Hat-based systems. However, it does not handle software dependencies automatically. For this, higher-level tools like `yum` or `dnf` are used, which work on top of RPM to resolve and manage dependencies efficiently.

- **Debian-based Systems:**  
For comparison, Debian-based systems like Ubuntu use `.deb` files and the `apt` package manager for similar purposes.

---

## 3 Types of Package Installation in Red Hat

Red Hat-based systems offer multiple ways to install packages, each with its own advantages and use cases. Here’s an overview:

---
### 1. **RPM or YUM**

- **RPM (Red Hat Package Manager):**
  - `rpm` is the core tool for managing `.rpm` packages.
  - **Pros:**  
    - Provides flexibility and control over package installation.  
    - Can install specific versions of software directly.  
  - **Cons:**  
    - Does not handle dependencies automatically, meaning you have to manually identify and install any missing dependencies.  
    - Recommended for advanced users or specific scenarios where full control is needed.  

    **Example Command:**
    ```bash
    rpm -ivh package_name.rpm
    ```

- **YUM (Yellowdog Updater, Modified):**
  - `yum` is a higher-level package manager that works on top of RPM and handles dependencies automatically.
  - **Pros:**  
    - Simplifies package management by automatically resolving and installing dependencies.  
    - Easier for beginners and suitable for general use.  
  - **Cons:**  
    - Requires access to repositories containing the necessary packages and their dependencies.

    **Example Command:**
    ```bash
    yum install package_name
    ```

---

### 2. **Source Code Compilation**

- Installing software by compiling source code is a more flexible and universal method, especially for software not available as `.rpm` or `.deb` packages.  
- **Steps Involved:**
  1. Download the source code (usually a `.tar.gz` or `.tar.bz2` file).
  2. Extract the archive:
     ```bash
     tar -xvzf package_name.tar.gz
     ```
  3. Configure the installation (check for dependencies):
     ```bash
     ./configure
     ```
  4. Compile the source code:
     ```bash
     make
     ```
  5. Install the compiled package:
     ```bash
     sudo make install
     ```

- **Pros:**  
  - Allows fine-tuning of the software for specific hardware or requirements.
  - Useful for software not included in repositories.

- **Cons:**  
  - Time-consuming and requires technical knowledge.
  - Dependency resolution must be managed manually.

---

### 3. **SRPM Installation**

- **SRPM (Source RPM):**  
  SRPM packages contain the source code and build instructions for creating binary `.rpm` packages.
  - Typically used by developers or advanced users who want to modify the source code before installation.
  - **Steps to Build and Install an SRPM:**
    1. Install the SRPM:
       ```bash
       rpm -ivh package_name.src.rpm
       ```
    2. Build the binary package:
       ```bash
       rpmbuild -ba /path/to/specfile.spec
       ```
    3. Install the resulting `.rpm` file:
       ```bash
       rpm -ivh package_name.rpm
       ```

- **Pros:**  
  - Enables customization of packages.  
  - Ensures compatibility with specific system configurations.

- **Cons:**  
  - More complex than using precompiled `.rpm` files.  
  - Requires knowledge of the `rpmbuild` process and tools.

---

### **DNF (Dandified Yum):**  

- In newer Red Hat systems, `dnf` has replaced `yum` as the default package manager. It offers better dependency management, faster performance, and improved handling of metadata. **Example Command:**
  ```bash
  dnf install package_name
  ```

- **Enterprise Environments:**  
  In corporate settings, package management often involves:
  - **Custom Repositories:** Hosting internal `.rpm` files to meet organizational requirements.
  - **Configuration Management Tools:** Using tools like Ansible, Puppet, or Chef to automate package installation across multiple systems.

---

### **Installing Packages with `--nodeps`**

The `--nodeps` option in RPM (Red Hat Package Manager) allows you to install a package without checking for or installing its dependencies. This can be useful in scenarios where you are confident that the required dependencies are already installed or where you want to force the installation of a package despite dependency issues. However, this should be used cautiously, as it can lead to broken installations or software that doesn’t work correctly.

### **What Advantages Does RPM Have When It Cannot Install Dependencies?**

1. **Manual Dependency Management:**
    
    - RPM allows you to override dependency issues with the `--nodeps` option, giving you control over which packages to install.
2. **Offline Installation:**
    
    - Useful in environments without internet access, where dependencies are manually resolved and installed beforehand.
3. **Testing and Development:**
    
    - Developers can test packages in isolated or controlled environments without installing additional dependencies.
4. **Forcing Compatibility:**
    
    - Enables installation of packages in non-standard setups where the system may already have equivalent or alternative dependencies.

---
##### Q: Is there any way to run `.exe` files in Linux?

**Ans:** Yes, there are some workarounds to execute a `.exe` file in Linux. But it is only limited to small software's which does not require much RAM (otherwise high RAM usage will force software to crash). **WINE** & **CrossOver** are the good examples for running `.exe` files in Linux.

