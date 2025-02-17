___
## **Network File System (NFS) in Linux**

### Overview

The **Network File System (NFS)** allows users to access files and directories on a remote server as if they were local. It operates on a **client-server model**, requiring NFS server software on the host system and NFS client software on the accessing system.

---

### Key Points

- NFS uses port `2049`.
- It works with the [[Networking (V) - OSI Model - Protocols - Ports#UDP (User Datagram Protocol)|UDP protocol]].
- NFS does not require a username or password for access.
- It supports file sharing between **Linux-to-Linux** systems exclusively.

For cross-platform file sharing, services like **Samba** offer broader capabilities and will be discussed in future notes.

---

## **Configuring NFS: Client and Server Setup**

### Step 1: Hostname Configuration

Assign persistent hostnames for both the server and client:

```bash
hostnamectl hostname server.example.com --static  
hostnamectl hostname client.example.com --static  
```

### Step 2: Flat DNS Setup

Edit the `/etc/hosts` file on both systems to associate IP addresses with hostnames:  
**On the server:**

```bash
192.168.0.10 server.example.com server  
192.168.0.8 client.example.com client  
```

**On the client:**

```bash
192.168.0.10 server.example.com server  
192.168.0.8 client.example.com client  
```

Verify connectivity by pinging each system using the defined hostnames:

```bash
ping server.example.com  
ping client.example.com  
```

### Step 3: Verify NFS Utilities

Ensure `nfs-utils` is installed on both systems:

```bash
rpm -qa | grep "nfs-utils"  
yum install nfs-utils  
```

---

## **Server-Side Configuration**

1. **Start and Enable the NFS Service**
    
    ```bash
    systemctl enable --now nfs-server.service  
    ```
    
    Confirm the service is running and listening on port `2049`:
    
    ```bash
    netstat -tulpn | grep "2049"  
    ```
    
2. **Set Up a Shared Directory**
    
    - Create a directory to share:
        
        ```bash
        mkdir /opt/shared_directory  
        ```
        
    - Add the directory to `/etc/exports`:
        
        ```bash
        /opt/shared_directory *(sync)  
        ```
        
3. **Load the Export Configuration**
    
    ```bash
    exportfs -rv  
    ```
    

---

## **Client-Side Configuration**

1. **Verify Shared Directories on the Server**
    
    ```bash
    showmount -e server.example.com  
    ```
    
2. **Mount the Shared Directory**
    
    - Create a mount point:
        
        ```bash
        mkdir /mnt/shared_directory  
        ```
        
    - Mount the server's directory:
        
        ```bash
        mount server.example.com:/opt/shared_directory /mnt/shared_directory  
        ```
        

> **Note:** If any connection issues arise, check and configure the server-side firewall or temporarily disable it for troubleshooting.

---

## **Managing Permissions in NFS**

1. **Understanding Permissions**
    
    - By default, NFS directories are mounted with **read-only (ro)** permissions.
    - To allow write access, modify the `/etc/exports` entry:
        
        ```bash
        /opt/shared_directory *(sync,rw)  
        ```
        
    - Reload the exports configuration:
        
        ```bash
        exportfs -rv  
        ```
        
2. **Change Directory Permissions**  
    Ensure appropriate permissions for the shared directory:
    
    ```bash
    chmod 777 /opt/shared_directory  
    ```
    

---

## **Centralized Storage in Enterprises**

NFS is a foundational service in Linux for building **centralized storage solutions**, enabling seamless file sharing across enterprise environments.

---

### FAQ: Interview and Real-World Scenarios

1. **What are the key differences between NFS and Samba for file sharing?**  
    _(Hint: Cross-platform compatibility and protocol usage.)_
    
2. **Why is the `/etc/exports` file important, and how do you verify its configuration?**  
    _(Hint: Discuss `exportfs` and its role in NFS setup.)_
    
3. **What steps would you take if a client cannot access the NFS share?**  
    _(Hint: Troubleshoot DNS, firewall settings, and service status.)_
    
4. **Explain the role of Flat DNS in NFS configurations.**  
    _(Hint: `/etc/hosts` and its importance in small setups.)_
    
5. **How does NFS handle file locking, and what challenges arise in multi-client environments?**  
    _(Hint: Use of `lockd` and potential race conditions.)_
    

---