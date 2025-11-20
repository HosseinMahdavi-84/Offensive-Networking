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

# DNS Fundamentals – Attacker's Perspective

## What is DNS?

- Domain Name System (DNS) is the phonebook of the internet
- Translates human-readable domain names → IP addresses
- Critical network protocol running primarily over **UDP port 53** (sometimes TCP 53)

## Basic DNS Workflow

```text
You type:    google.com
Browser → OS → DNS Resolver → Returns 142.250.190.14
→ Connection established
<img src="/images/Untitled_Diagram.drawio7.png" alt="Offensive Networking" width="100%"/>

Common DNS Tools

dig google.com
dig memoryleaks.ir

ping google.com          # also triggers DNS resolution
nslookup google.com
host google.com

Term,Definition
DNS Query,"Request asking ""What is the IP for this domain?"""
Name Resolution,Process of converting domain name → IP address
DNS Client,Your machine (or stub resolver) sending the query
DNS Server,Server that answers DNS queries (also called Name Server)
Recursive Resolver,"Usually your ISP or public DNS (8.8.8.8, 1.1.1.1) that does the full lookup"
Authoritative NS,The final DNS server that holds the real records for a domain

Where Does Your Machine Get the DNS Server IP?
Two ways:

Automatically via DHCP (from modem/router or ISP)
Manually configured in Linux:

Bashcat /etc/resolv.conf
nameserver 8.8.8.8
nameserver 1.1.1.1
Pro tip: Change to public DNS for speed & privacy
Bashecho "nameserver 1.1.1.1" | sudo tee /etc/resolv.conf
DNS Resolution Chain (Real Example)
textmemoryleaks.ir
       ↓
Local cache /etc/hosts → No
       ↓
Local DNS (127.0.0.1 or systemd-resolved)
       ↓
Router/Modem (192.168.1.1)
       ↓
ISP or Public DNS (8.8.8.8)
       ↓
Root Servers (.)
       ↓
Top-Level Domain (.ir)
       ↓
Authoritative Name Servers for memoryleaks.ir
       ↓
Returns A record → IP address
Where Are Authoritative Name Servers Configured?

In your domain registrar or DNS provider panel
Namecheap, Cloudflare, GoDaddy, NIC.ir, etc.
You set NS records like:textns1.cloudflare.com
ns2.cloudflare.com

<img src="/images/photo_2024-09-19_16-44-33.jpg" alt="Offensive Networking" width="100%"/>

Quick Cheat Sheet – Common dig Commands
Bashdig google.com               # Full answer
dig google.com +short        # Just IP
dig google.com A +short      # IPv4
dig google.com AAAA +short   # IPv6
dig google.com MX +short     # Mail servers
dig google.com NS +short     # Name servers
dig google.com TXT +short    # TXT records (SPF, DKIM, etc.)

# Query directly from authoritative server (bypass cache)
dig @ns1.cloudflare.com google.com
Bonus: Manual DNS Override (/etc/hosts)
Fastest way to "spoof" DNS locally:
Bashsudo nano /etc/hosts

# Add line:
10.10.10.100    admin.target.com
127.0.0.1       evil.com
Now your browser will go to that IP without any real DNS query!