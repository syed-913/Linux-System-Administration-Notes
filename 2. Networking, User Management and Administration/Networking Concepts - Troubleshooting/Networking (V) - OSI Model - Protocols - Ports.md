___
# OSI Model

To understand networking, the most important concept is the OSI model. The OSI model represents a hierarchy of data being transferred. The transmission of data is a process, and that process is defined by the OSI model. Following are the 7 layers of the OSI model:

## 7. Application Layer

- This layer represents the data in a human-readable form.
- It provides interfaces and protocols for applications to access network services.
- Examples include HTTP, FTP, and SMTP for web browsing, file transfer, and email.

## 6. Presentation Layer

- This layer translates data formats so applications can read it.
- It handles data encryption, compression, and encoding.
- Ensures data is presented in a usable format for the application layer.

## 5. Session Layer

- Establishes, manages, and terminates communication sessions.
- Manages dialogues and synchronization between applications.
- Ensures data flows in an orderly way, resuming after interruptions.

## 4. Transport Layer

- Responsible for data flow control, error checking, and data integrity.
- Manages end-to-end communication and segmentation of data packets.
- Protocols include TCP and UDP for reliable or faster, connectionless delivery.

## 3. Network Layer

- Determines the best path for data transmission between devices.
- Handles logical addressing and routing of packets across networks.
- Protocols include IP for routing and ICMP for error messages.

## 2. Data Link Layer

- Ensures reliable data transfer between adjacent nodes.
- Responsible for physical addressing (MAC addresses) and error detection.
- Divided into two sublayers: Logical Link Control (LLC) and Media Access Control (MAC).

## 1. Physical Layer

- Defines the hardware elements of network communication.
- Manages the transmission and reception of raw bitstreams over a physical medium.
- Includes specifications for cables, switches, and electrical signals.

In modern times, this OSI model is compressed into a 4 layered TCP/IP model. There's no difference in both models except the TCP/IP is consisted of 4 layers unlike OSI model.

|    TCP/IP Model    |            OSI Model             |
| :----------------: | :------------------------------: |
|  **Application**   | Application Presentation Session |
|   **Transport**    |            Transport             |
|    **Internet**    |             Network              |
| **Network Access** |        Data Link Pysical         |
___
# Protocols

Protocols are sets of rules and standards that define how data is transmitted and received across networks. Each protocol is designed for specific functions, ensuring data integrity, security, and efficient communication between devices. In-depth, protocols establish a structured method of data exchange, allowing diverse systems to communicate reliably. They handle various aspects such as error checking, data formatting, and timing, enabling seamless connectivity in complex networks.

The following are some of the most commonly used protocols in industries:

## HTTP (Hypertext Transfer Protocol)
- Facilitates the transfer of web pages and resources over the internet.
- Used primarily for accessing websites in a browser, allowing users to view content from web servers.
- Operates on a request-response model, where a client requests data, and a server responds.

## HTTPS (Hypertext Transfer Protocol Secure)
- An extension of HTTP with added security features, including encryption via SSL/TLS.
- Ensures data transmitted between the client and server is secure, protecting user privacy.
- Commonly used for secure transactions like online banking, shopping, and login forms.

## FTP (File Transfer Protocol)
- Allows the transfer of files between a client and a server on a network.
- Provides functions for uploading, downloading, and managing files remotely.
- Often used in web development, data backup, and file sharing systems.

## SMTP (Simple Mail Transfer Protocol)
- Used for sending emails from a client to a server or between mail servers.
- Provides a reliable way to transmit messages over the internet, especially for outgoing mail.
- Works in conjunction with other protocols like IMAP or POP3 for complete email functionality.

## DNS (Domain Name System)
- Translates human-readable domain names (e.g., example.com) into IP addresses.
- Essential for navigating the internet, as it enables users to access websites using easy-to-remember names.
- Functions through a network of servers that store and resolve domain information.

## IP (Internet Protocol)
- The primary protocol for addressing and routing data packets between devices on a network.
- Defines IP addresses for devices, allowing data to be sent to specific locations.
- Works in conjunction with TCP and UDP to deliver data across the internet.

## TCP (Transmission Control Protocol)
- Provides reliable, connection-oriented communication between devices.
- Ensures data packets are delivered accurately and in the correct order.
- Used in applications where accuracy is crucial, such as web browsing and email.

## UDP (User Datagram Protocol)
- A connectionless protocol that enables fast transmission of data without error-checking.
- Commonly used in applications where speed is more critical than accuracy, like online gaming and video streaming.
- Unlike TCP, it doesn’t guarantee data order or delivery, making it faster but less reliable.

## IMAP (Internet Message Access Protocol)
- Used for retrieving and managing emails from a mail server.
- Allows users to access their email from multiple devices, with changes synchronized across all clients.
- Often preferred over POP3 as it supports advanced email management features.

## POP3 (Post Office Protocol 3)
- Another protocol for retrieving emails from a server but stores them locally on the client.
- Generally deletes messages from the server after download, limiting multi-device access.
- Simple but effective for users who primarily access email from one device.

## ICMP (Internet Control Message Protocol)
- Used for diagnostic and error-reporting functions in network communication.
- Helps network devices report issues like unreachable destinations, excessive delays, and routing problems.
- Commonly used in tools like `ping` and `traceroute` to test connectivity and network paths.

ICMP doesn’t manage data transfer itself but plays a critical role in maintaining and troubleshooting network health.

___
# [[Ports In-Depth|Ports]]

Ports are communication endpoints in computer networks that allow devices to exchange data and establish connections. Each port has a unique number between 0 and 65535, which helps the network distinguish between different types of traffic. An IP address is necessary to access a device over the network, and ports further specify which service or application to reach on that device. Without an IP address, accessing specific ports is impossible, making IPs essential for navigating the internet.

In simple terms, a **port** acts as an interface between a service and its client. Each protocol has a unique port number assigned to its endpoint, helping route data to the appropriate application. Here are some of the most common ports used in networking:

| Port Number | Protocol               | Type        | Description                                                                                      |
| ----------- | ---------------------- | ----------- | ------------------------------------------------------------------------------------------------ |
| 20, 21      | FTP (File Transfer Protocol)   | TCP         | Used for file transfers between a client and server. Port 20 is for data, and 21 is for control.  |
| 22          | SSH (Secure Shell)     | TCP         | Provides secure remote access and command execution over encrypted channels.                      |
| 23          | Telnet                 | TCP         | Used for remote access and management of devices, but lacks encryption, making it less secure.    |
| 25          | SMTP (Simple Mail Transfer Protocol) | TCP | Handles sending emails from client to server or between mail servers.                             |
| 53          | DNS (Domain Name System) | UDP/TCP    | Resolves domain names to IP addresses, essential for internet navigation.                         |
| 80          | HTTP (Hypertext Transfer Protocol) | TCP | Used for transmitting web pages over the internet.                                                |
| 110         | POP3 (Post Office Protocol 3) | TCP       | Retrieves emails from a server, typically storing them locally on the client.                     |
| 143         | IMAP (Internet Message Access Protocol) | TCP | Enables email retrieval and management on a server, supporting multi-device access.               |
| 443         | HTTPS (Hypertext Transfer Protocol Secure) | TCP | Secures HTTP traffic with encryption, essential for secure online transactions.                   |
| 3389        | RDP (Remote Desktop Protocol) | TCP       | Allows remote access and control of a desktop over a network.                                     |

In case you forgot ports and its protocols/services view `/etc/services` to review the ports and protocols.

There are 2 types of ports:

### Privileged Ports
- Privileged ports ranges from 0 to 1023.
- These ports are most used ports.
- Also known as branded ports

### Non-Privileged Ports
- Non-Privileged Ports ranges from 1024 to 65535
- These are less used ports.
- Also known as Non-branded or Private Ports.
---
