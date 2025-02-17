___
## **Overview**
TCP Wrappers is a program that controls access to services in RHEL by filtering incoming network traffic. In simple terms, TCP Wrappers is a less functional type of **Firewall**, which is considered as **Application Firewall**. TCP Wrappers acts as an authenticating service between a client and a server application/service. It allows users to control access to wrapper network services, which are service compiled against the `libwrap.so` library.

#### **Configurations in TCP Wrappers**
To secure a Linux system with TCP Wrappers, there are two files which can be used to modify the permissions of individual host or even a whole network. I mean which host can access which service and which can't, same as for whole networks.

##### **`/etc/hosts.allow`**



##### **`/etc/hosts.deny`**


___
### **Limitations & Insecurities of TCP Wrappers**
There are multiple limitations of TCP Wrappers but the most common are mentioned below:
- **Server Entry:** When TCP Wrappers is in use then the service request from the host (outside the network) gets an entry in the server whice seems like a vulnerability or an insecurity of the server, although **its just a request to access the service** but it can cause other threat to the server. In simple terms, the request is blocked after entering the server **not outside the server**, what **firewall** does is that **it prevents the access to the server.**
- **Service Limitation:** Services which utilize `libwrap.so` socket in their background can be affected by the permissions set in`/etc/hosts.allow` and `/etc/hosts.deny` files. If a services doesn't include or utilize `libwrap.so` file, than that service will grant access to those hosts which are denied in the `/etc/hosts.deny` file because it doesn't get affected by `/etc/hosts.deny` file.
	- To check which service utilize `libwrap.so` file:
	```bash
	ldd <service_name> | grep "libwrap"
	```
