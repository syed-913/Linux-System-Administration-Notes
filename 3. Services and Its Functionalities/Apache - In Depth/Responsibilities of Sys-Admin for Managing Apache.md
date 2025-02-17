___
### **Common Sysadmin Tasks for Apache (`httpd`):**

#### 1. **Installing Apache**:

```bash
# On RHEL-based systems:
sudo yum install httpd

# On Debian-based systems:
sudo apt install apache2
```

#### 2. **Starting, Stopping, and Enabling the `httpd` Service**:

```bash
# Start the service
sudo systemctl start httpd

# Enable the service to start at boot
sudo systemctl enable httpd

# Check the status of the service
sudo systemctl status httpd
```

#### 3. **Configuring Apache**:

- Configuration files are stored in `/etc/httpd/` (RHEL) or `/etc/apache2/` (Debian).
- Key files include:
    - **Main Configuration**: `/etc/httpd/conf/httpd.conf`
    - **Virtual Hosts**: `/etc/httpd/conf.d/`

**Example: Setting up a Virtual Host**  
To host multiple websites on the same server:

```bash
sudo nano /etc/httpd/conf.d/example.com.conf
```

Add:

```apache
<VirtualHost *:80>
    ServerName example.com
    ServerAlias www.example.com
    DocumentRoot /var/www/example.com
    ErrorLog /var/log/httpd/example.com-error.log
    CustomLog /var/log/httpd/example.com-access.log combined
</VirtualHost>
```

#### 4. **Checking for Syntax Errors**:

Before restarting the server:

```bash
sudo apachectl configtest
```

#### 5. **Restarting the `httpd` Service**:

Apply configuration changes:

```bash
sudo systemctl restart httpd
```

#### 6. **Managing Logs**:

- Access logs: `/var/log/httpd/access_log`
- Error logs: `/var/log/httpd/error_log`
- Useful for debugging issues with websites or clients.

#### 7. **Securing Apache**:

- Enable SSL (HTTPS):
    
    ```bash
    sudo yum install mod_ssl
    sudo systemctl restart httpd
    ```
    
- Use a free SSL certificate from Let’s Encrypt:
    
    ```bash
    sudo certbot --apache
    ```
    

#### 8. **Troubleshooting Common Issues**:

- Check if the service is running:
    
    ```bash
    systemctl status httpd
    ```
    
- Ensure the firewall allows HTTP/HTTPS traffic:
    
    ```bash
    sudo firewall-cmd --add-service=http --permanent
    sudo firewall-cmd --add-service=https --permanent
    sudo firewall-cmd --reload
    ```
    
- Verify ports are open:
    
    ```bash
    sudo netstat -tuln | grep 80
    ```
    

---
