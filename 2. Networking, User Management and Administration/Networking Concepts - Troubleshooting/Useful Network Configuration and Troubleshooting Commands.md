___
### 1. **`lspci -vvv`** 
   - **Purpose**: Lists all PCI devices in your system, showing detailed information about hardware connected via PCI slots (e.g., network cards, graphics cards).
   - **Use case**: Helpful for identifying hardware devices or troubleshooting hardware issues.
   - **Example**:
     ```bash
     lspci -vvv
     ```

### 2. **`ethtool eth0`**
   - **Purpose**: Displays detailed settings of a network interface card (NIC). Use this command to check speed, duplex settings, and more.
   - **Use case**: Useful for diagnosing NIC-related issues, like speed or duplex mismatches, or checking NIC status.
   - **Example**:
     ```bash
     ethtool eth0
     ```
   - **Key Info**: Check for duplex settings, speed, and whether the NIC is up or down.

### 3. **`route -n`**
   - **Purpose**: Displays the system's routing table with network paths. The `-n` flag prevents name resolution (quicker output).
   - **Use case**: Important for troubleshooting network connectivity, checking routing and gateway configurations.
   - **Example**:
     ```bash
     route -n
     ```

### 4. `dig`
- **Purpose**: Perform detailed DNS lookups, especially useful for troubleshooting or analyzing DNS records.
- **Use**: `dig` is a powerful tool for querying various DNS record types, including `A`, `MX`, `CNAME`, and `TXT`.
- **Example**:
  ```bash
  dig google.com
  ```
  **Output**:
  ```
  ;; ANSWER SECTION:
  google.com.     299 IN  A  142.250.64.110
  ```

### 5. `nslookup`
- **Purpose**: Resolve a domain name to an IP address or get DNS information for a domain.
- **Use**: Widely used for quick lookups and basic troubleshooting of DNS resolution issues.
- **Example**:
  ```bash
  nslookup google.com
  ```
  **Output**:
  ```
  Server:         8.8.8.8
  Address:        8.8.8.8#53
  Non-authoritative answer:
  Name:   google.com
  Address: 142.250.64.110
  ```

### 6. `ping`
- **Purpose**: Test connectivity and check if DNS resolution is successful for a given domain.
- **Use**: Although primarily a network troubleshooting tool, `ping` also verifies that a domain name resolves correctly to an IP address.
- **Example**:
  ```bash
  ping google.com
  ```
  **Output**:
  ```
  PING google.com (142.250.64.110) 56(84) bytes of data.
  64 bytes from 142.250.64.110: icmp_seq=1 ttl=118 time=20.5 ms
  ```

---

## **General Networking Tips**
- **Check Network Configuration**: Use `ifconfig` or `ip addr` to view current network configurations.
- **Reapply Configuration**: After making changes to network settings, restart NetworkManager or bring the connection down and back up with `nmcli`.

---
