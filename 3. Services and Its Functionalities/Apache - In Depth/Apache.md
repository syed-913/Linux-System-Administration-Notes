___
Apache is a web-hosting server, which enables an `index.html` file to be displayed as a web-page on the browser. It is an open-source web server software developed by the **Apache Software Foundation**. It’s one of the most widely used web servers, known for its flexibility, extensibility, and reliability.
#### **Key Features of Apache:**

1. **Open Source**: Freely available and widely supported by the community.
2. **Cross-Platform**: Runs on various operating systems, including Linux, Windows, macOS, and Unix.
3. **Modular Architecture**: Apache uses modules to extend functionality (e.g., SSL, proxying, URL rewriting).
4. **Customizable Configuration**: Allows fine-grained control over virtual hosts, logging, access control, and more.
5. **Support for Dynamic Content**: Works seamlessly with scripting languages like PHP, Python, or Perl.
___
### **What is `httpd`?**

- `httpd` stands for **HTTP Daemon**, the process that runs in the background to serve web requests.
- When someone refers to "Apache running," they’re often talking about the `httpd` service.

#### **Role of `httpd`:**

1. **Host Websites**: Serves static files (HTML, images) and dynamic content (via PHP or other scripting languages).
2. **Handles HTTP Requests**: Listens on specific ports (default is port 80 for HTTP, 443 for HTTPS).
3. **Processes Incoming Requests**:
    - Matches requests to virtual hosts or files.
    - Sends back the appropriate response (e.g., a webpage or a 404 error).

#### **Interaction with DNS:**

- While `httpd` serves the website, **DNS** translates domain names (like `example.com`) into the IP address of the server where `httpd` is running.
___
### **Apache vs. `httpd`**

|**Aspect**|**Apache** (Apache HTTP Server)|**httpd** (Daemon)|
|---|---|---|
|**Definition**|A software/web server package.|The process running the Apache service.|
|**Function**|Manages configuration, modules, etc.|Executes requests as a background process.|
|**Relation**|Apache uses `httpd` as its core daemon.|`httpd` is part of the Apache server.|
___
### **How Website is accessed from browser?**
- The domain name (in our case`example.com`) is resolved in to the server's static(public) IP address.
- Connection is made to the Apache server on the respective ports `80`(http) or `443`(https) assigned in the `/etc/httpd/conf/httpd.conf` file. And `ServerName` is checked inside `<VirtualHost>` block.
- The `DocumentRoot` path defined inside `<VirtualHost>` block is checked and the files inside `DocumentRoot` (Default:`/var/www/html`) directory are displayed on the web page through Apache server.
___
### **1. Name-Based Hosting**

- **Definition**: Hosting multiple websites on a single IP address, differentiated by domain names.
    
- **How It Works**:
    
    - The browser includes the requested domain name in the `Host` header of the HTTP request.
    - The web server inspects this header to determine which site to serve.
- **Configuration Example**:
    
    - In Apache, you set up **Name-Based Virtual Hosts** like this:
        
        ```apache
        <VirtualHost *:80>
            ServerName example.com
            DocumentRoot /var/www/example.com
        </VirtualHost>
        
        <VirtualHost *:80>
            ServerName example.org
            DocumentRoot /var/www/example.org
        </VirtualHost>
        ```
        
        - Both sites share the same IP (`*` means "any IP").
- **Advantages**:
    
    - Efficient use of IP addresses.
    - Easier to scale for hosting multiple websites.
- **Disadvantages**:
    
    - Requires DNS resolution to map domain names to the server's IP address.
    - Cannot differentiate between websites when using SSL/TLS without **SNI** (Server Name Indication). SNI is now widely supported, but older browsers or clients may lack compatibility.

---

### **2. IP-Based Hosting**

- **Definition**: Hosting each website on a dedicated IP address, regardless of the domain name.
    
- **How It Works**:
    
    - Each website is tied to a specific IP address. The web server determines which site to serve based on the destination IP in the HTTP request.
- **Configuration Example**:
    
    - In Apache, you set up **IP-Based Virtual Hosts** like this:
        
        ```apache
        <VirtualHost 192.168.1.100:80>
            ServerName example.com
            DocumentRoot /var/www/example.com
        </VirtualHost>
        
        <VirtualHost 192.168.1.101:80>
            ServerName example.org
            DocumentRoot /var/www/example.org
        </VirtualHost>
        ```
        
        - Each `VirtualHost` listens on a specific IP address.
- **Advantages**:
    
    - Simplifies SSL/TLS setup when not using SNI.
    - Useful for legacy systems or applications that require a dedicated IP.
- **Disadvantages**:
    
    - Requires multiple IP addresses, which may be expensive or limited.
    - Not scalable for hosting a large number of websites.

---

### **When to Use Name-Based vs. IP-Based Hosting**

|**Use Case**|**Recommendation**|
|---|---|
|Hosting multiple sites|Use **Name-Based Hosting** to conserve IP addresses.|
|Legacy applications|Use **IP-Based Hosting** if they don't support SNI.|
|SSL/TLS without SNI|Use **IP-Based Hosting** or ensure SNI is supported.|
|Unique IP requirements|Use **IP-Based Hosting** for apps that require it.|

---

### **Modern Hosting Trends**

- **SNI Support**: Most modern clients and browsers now support SNI, making **Name-Based Hosting** viable for SSL/TLS.
- **Cloud Providers**: Services like AWS and Azure often default to Name-Based Hosting, with load balancers providing SSL termination.

---
### **Conclusion**:

Apache is the **web server software**, and `httpd` is the **daemon** responsible for running the Apache service in the background. Together, they allow sysadmins to host websites by processing HTTP requests and delivering content over the web. Understanding their interaction and configuration is key to effectively managing web servers on Linux.