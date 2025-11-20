# Networking Basics for Offensive Security

## Why Learn Networking as a Pentester / Bug Bounty Hunter?

- The entire internet is one giant interconnected network
- Discover advanced vulnerabilities (HTTP Request Smuggling, DESYNC attacks, etc.)
- Bypass firewalls, WAFs, IDS/IPS and other security devices
- Perform large-scale reconnaissance in bug bounty programs
- Exploit protocol weaknesses and misconfigurations effectively

## Core Terminology

| Term     | Definition                                                  |
|----------|--------------------------------------------------------------|
| Bypass   | Circumventing a security mechanism                           |
| Exploit  | Taking advantage of a vulnerability to gain unauthorized access |

## Fundamental Concepts

| Concept   | Description                                                                 |
|-----------|-----------------------------------------------------------------------------|
| Protocol  | Set of rules that allows computers to communicate with each other           |
| Client    | Device that requests resources or services                                  |
| Server    | Device that provides resources or services                                  |
| Internet  | Global network consisting of millions of clients and servers                |

## The OSI Model (7 Layers)

Introduced in 1984 to standardize communication between different systems.

| Layer | Name            | Responsibility                                                                 | Examples / Protocols                     |
|-------|-----------------|---------------------------------------------------------------------------------|------------------------------------------|
| 7     | Application     | Provides network services directly to end-user applications                     | HTTP, HTTPS, SSH, SMTP, DNS, FTP         |
| 6     | Presentation    | Data translation, encryption, compression                                       | SSL/TLS, JPEG, MPEG, character encoding  |
| 5     | Session         | Establishes, manages and terminates communication sessions                      | NetBIOS, RPC                             |
| 4     | Transport       | End-to-end delivery, segmentation, error control, flow control                 | TCP (reliable), UDP (fast)               |
| 3     | Network         | Logical addressing and routing between different networks                       | IP, ICMP, routers                        |
| 2     | Data Link       | Physical addressing and error detection on the local network                    | MAC addresses, Ethernet, ARP, switches   |
| 1     | Physical        | Transmission and reception of raw bit streams over physical medium             | Cables, Wi-Fi, hubs, repeaters           |

## Connections – How Devices Talk

- **IP Address** → Unique logical address to identify a device on the network  
  Example: `192.168.1.100` or `142.250.190.14`
- **Port** → Logical endpoint for communication (0–65535)  
  Well-known ports: 80 (HTTP), 443 (HTTPS), 22 (SSH), 53 (DNS)
- **Full Connection** = IP + Port  
  Example: `192.168.1.10:8080` or `example.com:443`

> Router's job: Find the best path and forward packets from source to destination

<img src="/images/Untitled_Diagram.drawio5.png" alt="Offensive Networking" width="100%"/>
## TCP vs UDP

| Feature                  | TCP (Transmission Control Protocol)               | UDP (User Datagram Protocol)      |
|--------------------------|----------------------------------------------------|------------------------------------|
| Connection               | Connection-oriented (3-way handshake)              | Connectionless                     |
| Reliability              | Guaranteed delivery + retransmission on failure    | No guarantee (fire and forget)     |
| Ordering                 | Preserves order using sequence numbers             | No ordering                        |
| Speed                    | Slower due to overhead                             | Very fast                          |
| Error Checking           | Yes                                                | Basic checksum only                |
| Common Use               | Web, email, file transfer, SSH                     | DNS queries, streaming, VoIP, games|
<img src="/images/Untitled_Diagram.drawio6.png" alt="Offensive Networking" width="100%"/>
### TCP 3-Way Handshake

```text
1. Client → Server : SYN
2. Server → Client : SYN-ACK
3. Client → Server : ACK
→ Connection successfully established