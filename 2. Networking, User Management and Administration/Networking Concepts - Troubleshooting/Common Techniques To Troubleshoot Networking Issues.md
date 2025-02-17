___
### **1. Testing Connectivity with `ping`**

- **Command**:
    
    ```bash
    ping <hostname or IP>
    ```
    
- **Use**:
    - Check basic network connectivity to a host.
    - Identify packet loss or high latency.
    - `ping -c 4 google.com` sends 4 ICMP packets to Google.

---

### **2. Checking Network Interface Status (`nmcli`, `ip` tools)**

- **Commands**:
    - View all interfaces:
        
        ```bash
        nmcli device status
        ```
        
    - Check IP addresses and details:
        
        ```bash
        ip addr show
        ```
        
    - Bring an interface up or down:
        
        ```bash
        nmcli device connect <interface_name>
        nmcli device disconnect <interface_name>
        ```
        
- **Use**:
    - Diagnose whether interfaces are active or properly configured.

---

### **3. Verifying Routing Table (`ip route`)**

- **Command**:
    
    ```bash
    ip route show
    ```
    
- **Use**:
    - Ensure the default gateway is set correctly.
    - Verify routing rules for specific subnets or hosts.

---

### **4. Managing DNS Resolution (`resolv.conf` or `nmcli`)**

- **Commands**:
    - Set a DNS server temporarily:
        
        ```bash
        echo "nameserver 8.8.8.8" >> /etc/resolv.conf
        ```
        
    - Permanently update DNS servers using NetworkManager:
        
        ```bash
        nmcli con mod <connection_name> ipv4.dns "8.8.8.8,8.8.4.4"
        nmcli con up <connection_name>
        ```
        
- **Use**:
    - Solve DNS-related issues (e.g., inability to resolve domain names).

---

### **5. Diagnosing Services with `telnet` or `nc` (Netcat)**

- **Commands**:
    - Test if a service is reachable on a specific port:
        
        ```bash
        telnet <hostname or IP> <port>
        ```
        
        OR
        
        ```bash
        nc -zv <hostname or IP> <port>
        ```
        
- **Use**:
    - Check if a service (e.g., web server or SSH) is accessible on a particular port.

---

### **6. Capturing and Analyzing Traffic (`tcpdump`, `Wireshark`)**

- **Commands**:
    - Capture packets for an interface:
        
        ```bash
        tcpdump -i <interface_name>
        ```
        
    - Filter packets for a specific port:
        
        ```bash
        tcpdump -i <interface_name> port 80
        ```
        
- **Use**:
    - Diagnose low-level network issues, such as packet drops or protocol errors.

---

### **7. Viewing and Restarting NetworkManager**

- **Commands**:
    - Check NetworkManager status:
        
        ```bash
        systemctl status NetworkManager
        ```
        
    - Restart NetworkManager:
        
        ```bash
        systemctl restart NetworkManager
        ```
        
- **Use**:
    - Restarting NetworkManager can resolve issues with misconfigured interfaces or connections.

---

### **8. Debugging Connections with `nmcli` and `nmtui`**

- **Commands**:
    - View detailed connection information:
        
        ```bash
        nmcli connection show <connection_name>
        ```
        
    - Use interactive mode to edit connections:
        
        ```bash
        nmtui
        ```
        
- **Use**:
    - Diagnose and fix issues with specific connections, such as incorrect IP, DNS, or gateway settings.

---

### **9. Inspecting Firewall Rules (`firewalld`, `iptables`)**

- **Commands**:
    - Check active firewall rules:
        
        ```bash
        firewall-cmd --list-all
        ```
        
    - Temporarily allow a port:
        
        ```bash
        firewall-cmd --add-port=80/tcp --zone=public --permanent
        firewall-cmd --reload
        ```
        
- **Use**:
    - Ensure the firewall isn’t blocking required traffic (e.g., SSH or HTTP).

---

### **10. Editing Network Scripts for Manual Configuration**

While NetworkManager automates most tasks, sometimes manual edits to configuration files are necessary:

- **Path**: `/etc/sysconfig/network-scripts/ifcfg-<interface_name>`
- Example File for a Static IP:
    
    ```bash
    TYPE=Ethernet
    BOOTPROTO=none
    IPADDR=192.168.1.100
    GATEWAY=192.168.1.1
    DNS1=8.8.8.8
    ONBOOT=yes
    ```
    
- Reload changes:
    
    ```bash
    nmcli con reload
    ```
    

---

### **Bonus Tip: Checking Logs**

- Use `journalctl` to inspect logs for networking issues:
    
    ```bash
    journalctl -u NetworkManager
    ```
    
