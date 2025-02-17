___

## **SUID (Set User ID)**

### **Overview**

The **Set User ID (SUID)** bit allows a user to execute a file with the permissions of the file's owner. This feature is essential for specific commands that require elevated privileges temporarily, such as changing a password using `/usr/bin/passwd`. This file, owned by `root`, has the SUID bit set, enabling non-root users to execute it and update their passwords.


### **Key Concepts**

- **SUID Representation in Permissions:**
    - `s`: SUID with executable permission (`-rws------`).
    - `S`: SUID without executable permission (`-rwS------`).
- **Octal Notation:** `4` represents SUID.

### **Enterprise Relevance**

SUID plays a critical role in managing user privileges while minimizing security risks. Misconfigured SUID bits can lead to privilege escalation vulnerabilities, often targeted in penetration testing.

### **Commands**

- **Set SUID (Symbolic):**
    
    ```bash
    chmod u+s /path/to/file
    ```
    
- **Set SUID (Octal):**
    
    ```bash
    chmod 4000 /path/to/file
    ```
    

---

## **GID (Set Group ID)**

### **Overview**

The **Set Group ID (GID)** bit enables files or directories to inherit the group ownership of the directory they reside in. This is crucial for collaborative environments where users share directories.

### **Key Concepts**

- **GID Representation in Permissions:**
    - `s`: GID with executable permission (`-rwxr-s---`).
    - `S`: GID without executable permission (`-rwxr-S---`).
- **Octal Notation:** `2` represents GID.

### **Commands**

- **Set GID (Symbolic):**
    
    ```bash
    chmod g+s /path/to/file
    ```
    
- **Set GID (Octal):**
    
    ```bash
    chmod 2000 /path/to/file
    ```
    

### **Enterprise Relevance**

In enterprises, shared directories with GID settings streamline collaborative workflows by automatically assigning the correct group permissions.

---

## **Sticky Bit**

### **Overview**

The **Sticky Bit** is used on directories to ensure that only the file owner or root can delete or rename files, even if others have write permissions.

### **Key Concepts**

- **Sticky Bit Representation in Permissions:**
    - `t`: Sticky bit with executable permission (`drwxrwxrwt`).
    - `T`: Sticky bit without executable permission (`drwxrwxrwT`).
- **Octal Notation:** `1` represents the Sticky Bit.

### **Commands**

- **Set Sticky Bit (Symbolic):**
    
    ```bash
    chmod o+t /path/to/directory
    ```
    
- **Set Sticky Bit (Octal):**
    
    ```bash
    chmod 1000 /path/to/directory
    ```
    

### **Enterprise Relevance**

The sticky bit is extensively used in shared directories like `/tmp` to prevent accidental or unauthorized file deletion.

---

## **Sudoers**

### **Overview**

The **Sudoers** file, located at `/etc/sudoers`, governs which users or groups can execute specific commands with elevated privileges. Editing this file directly can risk system stability; instead, use `visudo` to make changes safely.

### **Command Syntax**

- Open the Sudoers file:
    
    ```bash
    visudo
    ```
    
- Add user-specific permissions:
    
    ```bash
    <username> ALL=(ALL) NOPASSWD: /path/to/command
    ```
    

### **Enterprise Relevance**

In corporate environments, sudoers settings are vital for limiting root access and adhering to the principle of least privilege. Templates and predefined roles streamline permission management across teams.

### **Example**

Allow `umar` to manage the Apache service:

```bash
umar ALL=(ALL) NOPASSWD: /usr/bin/systemctl start, /usr/bin/systemctl stop, /usr/bin/systemctl restart
```

---

## **Set FACL (Access Control Lists)**

### **Overview**

**FACL** extends the standard permission system, enabling more granular control over file and directory access.

### **Commands**

- **View ACL:**
    
    ```bash
    getfacl /path/to/file
    ```
    
- **Set ACL:**
    
    ```bash
    setfacl -m u:username:rw /path/to/file
    ```
    
- **Remove ACL:**
    
    ```bash
    setfacl -x u:username /path/to/file
    ```
    
- **Default ACL for Directory:**
    
    ```bash
    setfacl -d -m u:username:rw /path/to/directory
    ```
    

### **Enterprise Relevance**

ACLs allow organizations to enforce strict compliance by precisely controlling access rights across departments.

---

## **Umask**

### **Overview**

**Umask** defines default file and directory permissions during creation. It’s pivotal in enforcing security policies by restricting unnecessary permissions.

### **Enterprise Relevance**

Umask ensures secure permission defaults for files created by automated processes or scripts in shared environments.

### **Practical Commands**

- View Current Umask:
    
    ```bash
    umask
    ```
    
- Set New Umask:
    
    ```bash
    umask 0027
    ```
    
- Persist Changes: Add the umask value to `/etc/profile` or `~/.bashrc`.

---

## **$PATH in Linux**

### **Overview**

The `$PATH` environment variable lists directories where executable files are searched for commands.

### **Practical Commands**

- Add a New Path (Temporary):
    
    ```bash
    export PATH=$PATH:/opt/myapp/bin
    ```
    
- Persist Changes: Add the path to `~/.bashrc` or `/etc/profile`.

### **Enterprise Relevance**

Proper `$PATH` management reduces conflicts, especially in environments with custom tools and multiple versions of software.

---

## **FAQ**

### **SUID, GID, and Sticky Bit**

1. **Q:** How to identify all SUID files on a system? **A:**
    
    ```bash
    find / -perm -4000
    ```
    
2. **Q:** How does the Sticky Bit enhance security in shared directories? **A:** Prevents unauthorized deletion of files by non-owners.
    

### **Sudoers**

3. **Q:** What’s the difference between `sudo` and `su`? **A:** `sudo` executes a command as another user, while `su` switches to another user session.
    
4. **Q:** How do you revoke a user's sudo privileges? **A:** Remove the user’s entry from `/etc/sudoers` or remove them from the sudo group:
    
    ```bash
    gpasswd -d username sudo
    ```
    

---