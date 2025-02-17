___
# Networking (IV) - IP Aliasing (Manual)

In Red Hat 7 and newer (using `systemd`), **IP aliasing** is the practice of assigning multiple IP addresses to a single network interface. This allows a single interface to handle multiple IPs, which is useful for scenarios such as virtual hosting, server consolidation, and network segmentation.

Here’s a detailed breakdown of how IP aliasing works in Red Hat 7+ with `systemd`:

---

### 1. **Understanding IP Aliasing Basics**

   - **IP aliasing** enables an interface (like `eth0`) to have multiple IP addresses (e.g., `192.168.1.10` and `192.168.1.11`), referred to as **virtual interfaces**.
   - Each alias is treated like a separate network interface, typically using the format `eth0:0`, `eth0:1`, etc., in traditional network configuration. However, in `systemd`, this setup is managed through **network unit files**.

---

### 2. **Configuring IP Aliasing with Systemd Network Files**

   Since Red Hat 7 introduced `systemd`, the network configuration can be managed in the following ways:

   - **Network scripts**: Although Red Hat 7 uses `systemd`, you can still create alias configurations under `/etc/sysconfig/network-scripts/`.
   - **Network Manager**: Alternatively, NetworkManager handles IP aliasing with a more dynamic configuration.
   - **Network Unit Files**: In `systemd`-based systems, you can set up additional IP addresses for an interface by using network unit files in `/etc/systemd/network/`.

---

### 3. **Configuring IP Aliases Using NetworkManager (Preferred)**

   - **Step 1:** Go to the directory for network scripts:
     ```bash
     cd /etc/sysconfig/network-scripts/
     ```

   - **Step 2:** Identify the network interface you want to alias, such as `ifcfg-eth0`.

   - **Step 3:** Create a new configuration file for each IP alias. For example:
     ```bash
     cp ifcfg-eth0 ifcfg-eth0:0
     ```

   - **Step 4:** Edit `ifcfg-eth0:0` and add the IP address and alias settings:
     ```ini
     DEVICE="eth0:0"
     BOOTPROTO=static
     IPADDR=192.168.1.10
     NETMASK=255.255.255.0
     ONBOOT=yes
     ```

   - **Step 5:** Restart the network service to apply changes:
     ```bash
     systemctl restart network
     ```

---

### 4. **Configuring IP Aliases with Network Unit Files in Systemd**

For a more `systemd`-native approach, configure alias addresses directly in `/etc/systemd/network/`.

   - **Step 1:** Create a new `.network` file for each alias under `/etc/systemd/network/`.
     ```bash
     sudo nano /etc/systemd/network/eth0.network
     ```

   - **Step 2:** Configure multiple IP addresses in the same unit file:
     ```ini
     [Match]
     Name=eth0

     [Network]
     Address=192.168.1.10/24
     Address=192.168.1.11/24
     Gateway=192.168.1.1
     ```

   - **Step 3:** Restart `systemd-networkd`:
     ```bash
     systemctl restart systemd-networkd
     ```

---

### 5. **Verifying IP Aliases**

Use the `ip` or `ifconfig` commands to check if aliases are applied:

```bash
ip addr show eth0
```

or

```bash
ifconfig eth0
```

You should see multiple IP addresses listed under the same interface.

---

### 6. **Benefits and Use Cases of IP Aliasing**

   - **Load balancing and failover**: Using multiple IPs allows different services to run on separate IPs on the same physical machine.
   - **Virtual hosting**: Useful for web servers hosting multiple websites that need unique IPs.
   - **Network segmentation**: Can logically separate traffic or applications without additional hardware.

---

**Note**: Although aliasing allows multiple IPs per interface, if extensive aliasing is needed, consider implementing VLANs for scalability and better traffic management.

