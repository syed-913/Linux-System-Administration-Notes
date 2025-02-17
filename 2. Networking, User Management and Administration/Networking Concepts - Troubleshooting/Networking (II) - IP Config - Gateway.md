___
### IP Configuration in Linux

When configuring IP settings in Linux, understanding the persistence of changes is crucial. The `ifconfig` command allows temporary configuration changes that reset after a reboot. To make configurations persistent, modify the relevant network configuration files, typically located in `/etc/NetworkManager/system-connections/` for NetworkManager-managed systems or `/etc/network/interfaces` for older systems.

#### Persistent IP Configuration Example

For NetworkManager:

1. Locate the desired connection profile:
   ```bash
   sudo nmcli connection show
   ```
2. Edit the profile using `nmcli`:
   ```bash
   sudo nmcli connection modify <connection_name> ipv4.addresses <IP_Address/Subnet> ipv4.gateway <Gateway_IP> ipv4.method manual
   ```
3. Restart the connection:
   ```bash
   sudo nmcli connection up <connection_name>
   ```

For systems using `/etc/network/interfaces`:

```plaintext
# Example configuration in /etc/network/interfaces
iface eth0 inet static
    address 192.168.1.100
    netmask 255.255.255.0
    gateway 192.168.1.1
```

After making changes, restart networking services:

```bash
sudo systemctl restart networking
```

---

### Gateway

A **gateway** serves as the network's exit point, routing data to external networks such as the internet. All devices on the same network segment can communicate directly without a gateway. However, a gateway is essential for traffic destined for other networks.

#### Viewing and Configuring Gateway

1. **View the current gateway:**
   ```bash
   route -n
   ```
   or:
   ```bash
   ip route
   ```
2. **Set a default gateway:**
   ```bash
   sudo route add default gw <Gateway_IP>
   ```
   Modern method:
   ```bash
   sudo ip route add default via <Gateway_IP>
   ```

To make these changes persistent, edit network configuration files as described in the previous section.

---

### Full and Half Duplex

Network duplex modes determine how data transmission occurs between two devices:

- **Full Duplex:**

  - Allows simultaneous sending and receiving of data.
  - Common in modern Ethernet networks, providing improved performance by eliminating collisions.

- **Half Duplex:**

  - Data transmission occurs in one direction at a time.
  - Used in older networks or specific low-bandwidth scenarios.
  - Susceptible to collisions, causing delays in communication.

#### Checking Duplex Mode

Use `ethtool` to inspect and change duplex settings:

```bash
ethtool eth0
```

---

### FAQs

#### Q: What happens if the gateway is misconfigured?

**A:** Devices may fail to reach external networks, even if local communications are unaffected. This issue often arises when multiple gateways are defined without proper prioritization.

#### Q: Can a gateway be part of a subnet?

**A:** Yes, a gateway typically resides within the same subnet as the devices it serves, enabling proper routing and communication.

#### Q: How does NetworkManager simplify IP configuration?

**A:** NetworkManager abstracts manual file editing, allowing users to configure IP addresses, gateways, and DNS via commands (`nmcli`) or graphical interfaces, streamlining network management.

