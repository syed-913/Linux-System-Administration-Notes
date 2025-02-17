___
## **Overview**
A **Firewall** is a network security device that monitors and controls network traffic to protect against external threats. **Firewalls** use a set of security rules to decide whether to allow or block specific traffic. These security rules are defined by Sys-Admins.

#### **What Firewall Does in Context of TCP Wrappers**
Consider a network including multiple servers offering services individually under a single router. Enterprises use hardware/tangible firewall devices to prevent security risks professionally. These are **firewall** devices placed behind router to prevent blocked IP's from accessing the services, servers, devices and more. There are multiple companies which offer firewall devices. Some of them are:

1. Cisco
2. SonicWall
3. Fortigate
4. Juniper
5. Paloalto

Types of Firewall are discussed in detail in the note linked below:
[[Types of Firewalls - In Detail]]
___
### **`firewalld` vs `iptables`**

|**Aspect**|**iptables**|**firewalld**|
|---|---|---|
|**Definition**|Command-line utility for managing netfilter rules directly.|A dynamic firewall management tool that acts as a front-end for `iptables` or `nftables`.|
|**Rule Application**|Rules are applied statically; changes require reloading the entire ruleset.|Rules are applied dynamically without disrupting active connections.|
|**Interface**|Command-line focused, requiring manual rule definition.|User-friendly with CLI (`firewall-cmd`) and graphical interfaces.|
|**Ease of Use**|Steeper learning curve with complex manual configurations.|Simplified management using zones and predefined services.|
|**Zones**|No native concept of zones; rules are applied globally or for specific chains.|Supports zones (e.g., public, internal) for segregating and applying rules based on trust levels.|
|**Service Support**|Rules must explicitly define ports, protocols, and IPs.|Predefined services (e.g., HTTP, SSH) simplify configuration.|
|**Persistence**|Rules are not persistent unless explicitly saved and reloaded via scripts.|Persistent by default; runtime and permanent rules can be managed separately.|
|**Dynamic Changes**|Requires restarting `iptables` service to apply changes.|Allows dynamic modification of rules without restarting the firewall service.|
|**Backend**|Directly interfaces with the kernel's netfilter system.|Uses `iptables` (or optionally `nftables` in modern systems) as the backend.|
|**Compatibility**|Available on most Linux systems; traditionally used in pre-RHEL 7 systems.|Default firewall on RHEL 7+, CentOS 7+, Fedora, and similar distributions.|
|**Logging**|Requires manual setup for logging accepted/dropped packets.|Integrated logging with simpler configuration options.|
|**Best Use Cases**|Advanced users requiring granular control or environments with custom configurations.|General sysadmins or users needing dynamic, simplified firewall management.|
|**Example Command (Add Rule)**|`iptables -A INPUT -p tcp --dport 22 -j ACCEPT`|`firewall-cmd --zone=public --add-service=ssh --permanent`|
|**Example Command (Persistence)**|`service iptables save`|Rules are automatically persistent unless explicitly defined as runtime-only.|
### **Example Commands**

#### **iptables**

1. Add a rule to allow SSH:
    
    ```bash
    iptables -A INPUT -p tcp --dport 22 -j ACCEPT
    ```
    
2. Save rules for persistence:
    
    ```bash
    service iptables save
    ```
    

#### **firewalld**

1. Allow SSH in the public zone:
    
    ```bash
    firewall-cmd --zone=public --add-service=ssh --permanent
    ```
    
2. Reload rules:
    
    ```bash
    firewall-cmd --reload
    ```
    

---

### **Key Takeaways**

- **`iptables`**: Provides granular control but requires more effort and expertise to manage. Suitable for power users and custom environments.
- **`firewalld`**: Offers a higher-level abstraction with zones and dynamic management, making it ideal for most modern Linux distributions and general sysadmins.
