___
### **Firewall Zones**

1. **List all available zones**:
    
    ```bash
    firewall-cmd --get-zones
    ```
    
    - Displays predefined and custom zones.
2. **Check the active zones**:
    
    ```bash
    firewall-cmd --get-active-zones
    ```
    
    - Shows zones currently being used and their associated interfaces.
3. **Assign an interface to a zone**:
    
    ```bash
    firewall-cmd --zone=<zone> --change-interface=<interface>
    ```
    
    - Example:
        
        ```bash
        firewall-cmd --zone=public --change-interface=eth0
        ```
        
4. **Set a default zone**:
    
    ```bash
    firewall-cmd --set-default-zone=<zone>
    ```
    
    - Example:
        
        ```bash
        firewall-cmd --set-default-zone=public
        ```
        

---

### **Managing Services**

5. **List all services provided by firewalld**:
    
    ```bash
    firewall-cmd --get-services
    ```
    
6. **Add a service to a zone**:
    
    ```bash
    firewall-cmd --zone=<zone> --add-service=<service>
    ```
    
    - Example:
        
        ```bash
        firewall-cmd --zone=public --add-service=http
        ```
        
7. **Remove a service from a zone**:
    
    ```bash
    firewall-cmd --zone=<zone> --remove-service=<service>
    ```
    
    - Example:
        
        ```bash
        firewall-cmd --zone=public --remove-service=http
        ```
        
8. **List active services for a zone**:
    
    ```bash
    firewall-cmd --zone=<zone> --list-services
    ```
    

---

### **Managing Ports**

9. **Open a port**:
    
    ```bash
    firewall-cmd --zone=<zone> --add-port=<port>/<protocol>
    ```
    
    - Example:
        
        ```bash
        firewall-cmd --zone=public --add-port=8080/tcp
        ```
        
10. **Close a port**:
    
    ```bash
    firewall-cmd --zone=<zone> --remove-port=<port>/<protocol>
    ```
    
    - Example:
        
        ```bash
        firewall-cmd --zone=public --remove-port=8080/tcp
        ```
        
11. **List active ports in a zone**:
    
    ```bash
    firewall-cmd --zone=<zone> --list-ports
    ```
    

---

### **Permanent Rules**

12. **Make changes permanent**:
    
    - Add `--permanent` to any command to persist changes across reboots.
    - Example:
        
        ```bash
        firewall-cmd --zone=public --add-port=8080/tcp --permanent
        ```
        
13. **Reload the firewall after permanent changes**:
    
    ```bash
    firewall-cmd --reload
    ```
    

---

### **Rich Rules (Advanced Rules)**

14. **Add a rich rule**:
    
    ```bash
    firewall-cmd --zone=<zone> --add-rich-rule='<rule>'
    ```
    
    - Example:
        
        ```bash
        firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="192.168.1.100" port port="22" protocol="tcp" accept'
        ```
        
15. **Remove a rich rule**:
    
    ```bash
    firewall-cmd --zone=<zone> --remove-rich-rule='<rule>'
    ```
    

---

### **Logging and Monitoring**

16. **Enable logging for dropped packets**:
    
    ```bash
    firewall-cmd --set-log-denied=all
    ```
    
17. **View runtime configurations**:
    
    ```bash
    firewall-cmd --list-all
    ```
    

---

### **Miscellaneous**

18. **Block all incoming traffic (panic mode)**:
    
    ```bash
    firewall-cmd --panic-on
    ```
    
19. **Disable panic mode**:
    
    ```bash
    firewall-cmd --panic-off
    ```
    
20. **Check firewall status**:
    
    ```bash
    firewall-cmd --state
    ```
    

---

### **Use Cases in Enterprises**

1. **Securing Web Servers**:
    
    - Open ports for HTTP/HTTPS:
        
        ```bash
        firewall-cmd --zone=public --add-service=http --permanent
        firewall-cmd --zone=public --add-service=https --permanent
        ```
        
2. **Allowing Database Access**:
    
    - Open ports for MySQL or PostgreSQL:
        
        ```bash
        firewall-cmd --zone=public --add-service=mysql --permanent
        ```
        
3. **Restricting SSH Access**:
    
    - Allow SSH from a specific IP:
        
        ```bash
        firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="203.0.113.10" service name="ssh" accept' --permanent
        ```
        
4. **Logging Suspicious Traffic**:
    
    - Enable and monitor logs for denied traffic:
        
        ```bash
        firewall-cmd --set-log-denied=all
        tail -f /var/log/messages | grep 'firewalld'
        ```
        

---