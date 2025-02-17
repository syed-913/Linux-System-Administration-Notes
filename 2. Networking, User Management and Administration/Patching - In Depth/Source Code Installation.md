___

We've discussed **RPM** and **YUM** package installers. Now, in the context of these installers, we explore **Source Code Installation** of a package.

**Source Code Installation** involves multiple steps, unlike YUM or RPM, where a single command usually suffices for installation (along with dependency resolution).

One key limitation of RPM and YUM is that we cannot modify or set custom configurations for a particular package. However, this is the **greatest advantage of Source Code Installation**, as it allows for customization and manual configuration.

---

## **Advantages of Source Code Installation**

- **Customizability**: Allows adding or removing features during installation.
- **Control Over Dependencies**: You can decide how dependencies are managed or integrated.
- **Optimization**: Custom compilation for specific hardware or performance needs.
- **Access to Latest Version**: Often provides the latest software version not available in package managers.

---

## **Steps of Source Code Installation**

> **Note:** Make sure you're in the directory containing the extracted tarball.

### **Step 1: Downloading**

- Browse the internet and download a tarball from a trusted source.
- Example: Downloading the `apache` tarball using `wget`:
    
    ```bash
    wget https://archive.apache.org/dist/httpd/httpd-2.4.9-deps.tar.gz
    ```
    

#### **Best Practices for Downloading**

- Always verify the source (e.g., official websites or trusted repositories).
- Check the checksum (e.g., MD5, SHA256) of the downloaded tarball to ensure its integrity:
    
    ```bash
    sha256sum httpd-2.4.9-deps.tar.gz
    ```
    

---

### **Step 2: Extracting**

- The downloaded tarball must be extracted to proceed with installation.
- Example: Extracting a tarball compressed with `bzip2`:
    
    ```bash
    tar -jxvf httpd-2.4.9-deps.tar.gz
    ```
    

#### **Additional Notes**

- The extraction creates a directory containing the source code.
- Navigate into this directory to proceed:
    
    ```bash
    cd httpd-2.4.9-deps
    ```
    

---

### **Step 3: Configure**

- In this step, you can configure custom options (if needed).
- The `configure` script checks for dependencies on the host system.
- Example:
    
    ```bash
    ./configure
    ```
    
    Or for custom configurations:
    
    ```bash
    ./configure --enable-php --enable-java
    ```
    

#### **Dependency Check**

- The `configure` script will throw errors if required dependencies are missing.
- Common dependencies for `apache` include:
    - `gcc` (C Compiler)
    - `make`
    - `apr`
    - `pcre`

Install missing dependencies using:

```bash
yum install gcc make apr pcre
```

---

### **Step 4: Compiling**

- Use the `make` command to compile all modules of the program:
    
    ```bash
    make
    ```
    

#### **Compilation Details**

- Compilation generates binary files from the source code.
- You can speed up the compilation process on multi-core systems by specifying the number of cores:
    
    ```bash
    make -j4  # Uses 4 cores
    ```
    

---

### **Step 5: Installing**

- Install the program by running:
    
    ```bash
    make install
    ```
    

#### **Customization of Installation Path**

- If you want to install the program in a custom directory (e.g., `/opt/apache`), specify the path during configuration:
    
    ```bash
    ./configure --prefix=/opt/apache
    ```
    
- This ensures the program doesn't interfere with system-wide files.

---

### **Step 6: Review**

- Verify that the installation was successful and check the installation directory.
- The program files are typically stored in `/usr/local/` or another specified directory.
- Example:
    
    ```bash
    ls /usr/local/apache
    ```
    

#### **Post-Installation Checks**

- Check the installed program’s version to confirm the installation:
    
    ```bash
    apachectl -v
    ```
    

---

### **Uninstallation**

- To uninstall a program installed via source code, simply delete the main directory (e.g., `/usr/local/<program-name>`).
- You may also need to remove related configurations manually.

---

### **Enterprise Context**

Source code installation is rarely used in enterprises due to its time-consuming nature and maintenance challenges. RPM and YUM are more flexible and efficient for enterprise needs.

However, source code installations are commonly used in:

- **Development Environments**: For testing new versions or experimental features.
- **Specialized Systems**: Where customization or optimization is required.

---

### **Comparison Table: RPM, YUM, and Source Code Installation**

| Feature                   | **RPM**                                      | **YUM/DNF**                        | **Source Code Installation**       |
| ------------------------- | -------------------------------------------- | ---------------------------------- | ---------------------------------- |
| **Dependency Resolution** | Manual                                       | Automatic                          | Manual (configure step identifies) |
| **Ease of Use**           | Moderate                                     | High                               | Low                                |
| **Customization**         | Not Supported                                | Not Supported                      | Fully Customizable                 |
| **Speed**                 | Fast                                         | Fast                               | Time-Consuming                     |
| **Installation Method**   | Precompiled binary                           | Precompiled binary                 | Compiles source code               |
| **Uninstallation**        | Automatic                                    | Automatic                          | Manual (delete main directory)     |
| **Usage in Enterprises**  | Common                                       | Very Common                        | Rare                               |
| **Error Handling**        | Errors may arise if dependencies are missing | Automatically handles dependencies | Errors must be fixed manually      |
___
