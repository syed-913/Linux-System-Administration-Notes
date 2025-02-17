___
## **Overview**
The Domain Name System (DNS) is the phonebook of the Internet. Humans access information online through [domain names](https://www.cloudflare.com/learning/dns/what-is-dns/), like nytimes.com or espn.com. Web browsers interact through Internet Protocol (IP) addresses. DNS translates domain names to IP addresses, so browsers can load Internet resources.

Each device connected to the Internet has a unique IP address which other machines use to find the device. DNS servers eliminate the need for humans to memorize IP addresses such as *192.168.1.1* (in IPv4), or more complex newer alphanumeric IP addresses such as *2400:cb00:2048:1::c629:d7a2* (in IPv6).
___
## **Why A private DNS server is Required in Enterprises**
When a client search's something with the domain name on the browser, then their Internet Service Provider (ISP) takes them to the open internet and connects them with the public DNS server on **port 53** like **google (8.8.8.8) or cloudflare (1.1.1.1)**. This seems more vulnerable for clients for various reasons. To avoid these public servers an enterprise requires an internal or private DNS server for their clients safety.
#### **How does Recursive + Caching DNS server Works?**

![[DNS Diagram.svg]]
This is how a client access the domains (or in other words hosts) on the internet. Both DNS servers (ISP Server & Private Server) works the same way but what differs both of them is privacy, control, and security the most important aspect in cyber security. Both ISP and Private server cache the most recursive domains but they both go out when the requested domain is not in their query list. Also if the ISP server goes down then there's no way to recover unless the ISP takes quick action but if we have our own DNS server than we can control it based on the enterprise's requirements.
___
## **Steps to Create a Recursive/Caching DNS Server**
To implement a DNS server we must require a package called `bind`. There are several other packages which can be used to deploy a DNS server bu for learning purposes we are using `bind`.
### **1. Install the Package**
- Just install the package with `yum` and list with `rpm`:
	```bash
	yum install bind-* -y
	rpm -qa | grep "bind"
	```
### **2. Start the DNS Service**
- Start the `named` service:
	```bash
	systemctl start named-chroot.service
	```
- Configure it if its running or not:
	```bash
	netstat -tulpn | grep "53" #Grab the DNS service port
	```
### **3. Configure Firewall**
- Give permission to the `named` service to access internet through firewall.
	```bash
	firewall-cmd --state
	firewall-cmd --permanent --add-service=dns
	firewall-cmd --reload
```
### **4. Move the Configuration file**
- I know this seems off a bit but there's no need of `/etc/named.conf` for learning purposes only.
```bash
	mv /etc/named.conf /opt/
```
___
### **Types of DNS in the Context of Enterprise DNS Servers**

DNS plays a pivotal role in enterprise networking by resolving domain names to IP addresses. Here's an explanation of various types of DNS with their relevance in enterprise scenarios:

---

#### 1. **Recursive DNS**

- **How it Works**:
    
    - A recursive DNS server takes a client's DNS query and performs all necessary lookups to resolve the query, eventually returning the final IP address to the client.
    - It queries authoritative DNS servers, starting from the root, to fetch the result.
- **Enterprise Perspective**:
    
    - Used as the first point of contact for employees or devices in an enterprise network.
    - Often provided by internal corporate DNS servers or third-party providers (e.g., Google Public DNS, Cloudflare, or ISP-managed).
    - Includes caching mechanisms to improve speed and reduce external traffic.
- **Use Cases**:
    
    - **Caching**: Reduces latency for frequently accessed domains.
    - **Content Filtering**: Enterprises use recursive servers with filtering to block malicious or non-business domains.

---

#### 2. **Authoritative DNS**

- **How it Works**:
    
    - An authoritative DNS server is responsible for answering queries about domains it manages.
    - It contains the original, definitive records (A, MX, TXT, etc.) for a specific domain.
- **Enterprise Perspective**:
    
    - Enterprises host authoritative DNS servers for their domains (e.g., company.com) to control DNS records and configurations.
    - Redundant setups across multiple regions ensure high availability and fault tolerance.
- **Use Cases**:
    
    - **Hosting Public Services**: Used for websites, email servers, and APIs.
    - **Load Balancing**: Integrates with services like Amazon Route 53 to direct traffic efficiently across servers.

---

#### 3. **Forwarding DNS**

- **How it Works**:
    
    - A forwarding DNS server relays queries to another DNS server (usually recursive) instead of resolving them itself.
- **Enterprise Perspective**:
    
    - Acts as an intermediary between internal clients and external DNS servers.
    - Reduces latency and improves management by centralizing external lookups.
- **Use Cases**:
    
    - **Policy Enforcement**: Implements corporate rules for DNS queries, such as blocking social media or non-business sites.
    - **Optimization**: Reduces external DNS traffic by leveraging a caching layer.

---

#### 4. **Local DNS (Land DNS)**

- **How it Works**:
    
    - A local DNS server resolves queries for internal domains (e.g., intranet.company.com) and forwards other requests to a recursive DNS server.
- **Enterprise Perspective**:
    
    - Used exclusively within an enterprise network to manage internal resources.
    - Integrated with Active Directory for environments that require Kerberos or NTLM authentication.
- **Use Cases**:
    
    - **Internal Name Resolution**: Resolves internal service names and server hostnames.
    - **Access Control**: Restricts DNS resolution to authorized internal clients.

---

#### 5. **Dynamic DNS (DDNS)**

- **How it Works**:
    
    - Updates DNS records in real-time to reflect changes in IP addresses, often for devices with dynamically assigned IPs.
- **Enterprise Perspective**:
    
    - Useful for environments with a large number of mobile or IoT devices that frequently change locations or IPs.
    - DDNS is commonly integrated with DHCP to update records automatically.
- **Use Cases**:
    
    - **IoT Management**: Tracks devices in a dynamic network environment.
    - **Remote Access**: Ensures remote employees can access resources even with changing IPs.

---

#### 6. **Public vs. Private DNS**

- **Public DNS**:
    
    - Resolves public domains and is accessible from the internet.
    - Example Providers: Google Public DNS (8.8.8.8), Cloudflare (1.1.1.1).
- **Private DNS**:
    
    - Resolves internal domains and is accessible only within the enterprise network.
    - Often linked to internal zones, such as `corp.company.com`.

---

#### 7. **Split-Brain DNS**

- **How it Works**:
    
    - Maintains separate DNS records for internal and external users of the same domain.
    - For example:
        - Internal: `web.company.com → 192.168.1.10`
        - External: `web.company.com → 203.0.113.10`
- **Enterprise Perspective**:
    
    - Essential for security and access control.
    - Enables different resolution paths for services based on user location.
- **Use Cases**:
    
    - **VPN**: Directs remote users through a secure path.
    - **Service Segmentation**: Keeps internal services hidden from external access.

---

### Best Practices for Enterprise DNS Servers:

1. **Redundancy**:
    - Deploy multiple DNS servers across regions and availability zones for fault tolerance.
2. **Caching**:
    - Enable caching to reduce external traffic and improve query response times.
3. **Security**:
    - Implement DNSSEC to prevent spoofing and cache poisoning.
    - Use private DNS zones for internal resources.
4. **Monitoring**:
    - Monitor DNS logs for suspicious activity, such as DNS tunneling attempts.
5. **Integration**:
    - Leverage solutions like AWS Route 53 or Azure DNS for seamless integration with cloud environments.
___
## **Naming System**

### Flat DNS

- **Description:** A static, manual method of mapping hostnames to IP addresses.
- **Management in Linux:** Through the `/etc/hosts` file.
- **Usage:** Suitable for local networks, testing, or environments with a small number of hosts.
- **Limitations:** Not scalable or dynamic, making it unsuitable for large or dynamic environments.

### Hierarchical DNS

- **Description:** A globally distributed system with a tree-like structure for resolving domain names.
- **Components:** Includes **root servers**, **TLDs (Top-Level Domains)**, and **authoritative name servers**.
- **Configuration in Linux:** Managed via `/etc/resolv.conf`.
- **Usage:** Ideal for cloud and enterprise infrastructures where IP addresses change frequently.

> **Note:** For small setups involving one client and one server, **Flat DNS** is more cost-effective and sufficient.

___
