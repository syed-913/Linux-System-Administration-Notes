## **SE Linux (Security Enhanced Linux)**

SE Linux is a **[[Kernel Management, Patching & Tuning|kernel security module]]** providing an additional layer of security through **Mandatory Access Control (MAC)**. It enhances the security of Linux systems by limiting the impact of vulnerabilities. SE Linux was developed by the **National Security Agency (NSA)** in 2000 and is now widely used in enterprise Linux distributions.

### **DAC vs MAC**
#### **Discretionary Access Control (DAC)**
- DAC is the default permission model in Linux.
- Access is granted based on the user’s ownership and permission flags like `rwxr-xr-x` (755) or `rw-r--r--` (644).
- Users can change permissions or escalate privileges if they have sufficient access, which makes DAC less secure.

#### **Mandatory Access Control (MAC)**
- MAC enforces security policies that are centrally defined and cannot be modified by individual users.
- Policies control access based on labels and rules, irrespective of DAC permissions.
- Ensures processes and users only access resources they are explicitly allowed to.

#### **How MAC is Better Than DAC**
- Provides fine-grained control over access to resources.
- Prevents privilege escalation by enforcing strict policies.
- Limits the scope of vulnerabilities by compartmentalizing processes and data.

---

## **SE Linux Modes**
SE Linux operates in three modes:
- **Enforcing:** SE Linux policies are enforced, and access violations are blocked (default in Red Hat-based distributions).
- **Permissive:** SE Linux policies are not enforced, but violations are logged for troubleshooting.
- **Disabled:** SE Linux is completely turned off, and no access control is applied.

### **Switching Modes**
- To check the current mode:
  ```bash
  getenforce
  sestatus
  ```
- To change the mode temporarily (non-persistent):
  ```bash
  setenforce 0  # Permissive
  setenforce 1  # Enforcing
  ```
- To make changes persistent:
  Modify `/etc/selinux/config`:
  ```bash
  SELINUX=permissive  # Options: enforcing, permissive, disabled
  ```

---

## **Labeling in SE Linux**
SE Linux uses labels (contexts) to enforce access control. Every file, process, and resource is assigned a **security context** comprising:
- **User:** User identity (e.g., `system_u`).
- **Role:** Role associated with the object (e.g., `object_r`).
- **Type:** Type of object (e.g., `httpd_sys_content_t`).

### **Key Concepts in Labeling**
- Labels are stored in `/etc/selinux/targeted/contexts/files/file_contexts`.
- Incorrect labels can cause permission issues.

#### **Relabeling Files**
If SE Linux is switched from `disabled` to `enforcing` or `permissive`, file labels may need to be corrected:
1. Create the `/.autorelabel` file and reboot for automatic relabeling.
2. Use the command:
   ```bash
   fixfiles -F onboot
   ```

#### **Changing File Labels**
To manually change the type label of a file or directory:
```bash
chcon -t <type> <filename>
```
Example:
```bash
chcon -t httpd_sys_content_t /var/www/html/index.html
```

---

## **SE Linux Boolean Values**
The term **enforcing** operates on **Booleans** in SE Linux allow administrators to toggle specific access controls for services, processes, or directories without modifying policies.

### **Managing Booleans**
- To list all booleans and their states:
  ```bash
  getsebool -a
  ```
- To modify a boolean temporarily:
  ```bash
  setsebool <boolean_name> on/off
  ```
- To make changes persistent:
  ```bash
  setsebool -P <boolean_name> on/off
  ```

#### **Example:**
Allowing an FTP server to access home directories:
```bash
setsebool -P ftp_home_dir on
```

---

## **Commands for SE Linux Management**

### **Displaying SE Linux Information**
- Check current mode:
  ```bash
  getenforce
  ```
- Display detailed status:
  ```bash
  sestatus
  ```

### **Modifying SE Linux Settings**
- Temporarily change mode:
  ```bash
  setenforce 0  # Permissive
  setenforce 1  # Enforcing
  ```
- Persistent configuration:
  Modify `/etc/selinux/config`:
  ```bash
  SELINUX=enforcing  # Options: enforcing, permissive, disabled
  ```

### **Logs and Troubleshooting**
- Check logs for SE Linux issues:
  ```bash
  journalctl -b 0 | grep SELinux
  ```
- Use `ausearch` to filter audit logs:
  ```bash
  ausearch -m avc
  ```
- Use `sealert` for detailed troubleshooting and solutions:
  ```bash
  sealert -a /var/log/audit/audit.log
  ```

---

## **SE Linux in Enterprise Environments**
### **Best Practices**
1. Use `enforcing` mode in production for maximum security.
2. Regularly audit and update SE Linux policies.
3. Test new configurations in `permissive` mode before enforcing.
4. Document custom rules and label changes for future reference.

### **Real-World Applications**
- Web servers: Protect web content and scripts with strict policies.
- Databases: Enforce process isolation and prevent unauthorized access.
- Multi-user environments: Prevent users from accessing each other’s data.

---

## **FAQ and Interview Questions**

### **Easter Egg Questions**
1. **Why was SE Linux developed by the NSA?**
   - It was developed as part of the NSA’s research into secure operating systems to prevent unauthorized data access and mitigate vulnerabilities.

2. **How does SE Linux enhance security compared to AppArmor?**
   - SE Linux uses MAC with fine-grained policies and centralized control, while AppArmor relies on path-based access controls.

3. **What is the difference between `chcon` and `restorecon`?**
   - `chcon` temporarily changes labels without modifying policy.
   - `restorecon` resets labels based on the default policy context.

4. **What happens if a process doesn’t have the correct type?**
   - SE Linux denies access and logs the violation, potentially breaking the application until the correct label is applied.

