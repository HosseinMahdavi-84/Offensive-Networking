# Network

## Why?

- The internet is an universal network
- To discover vulnerabilities (Smuggling, etc)
- To bypass security devices (Firewall, etc)
- To conduct wide recon in bug bounty
- To exploit vulnerabilities

## Terminology

- **bypass**: circumventing a security measure
- **exploit**: taking advantage of vulnerabilities

## Network

- **Protocol**: rules for computers to communicate (computer context)
- **Client**: a computer in network
- **Server**: a computer in network which serves data
- **Internet**: a big network (clients and servers)

## OSI Model

- Introduced in 1984
- Communication in dissimilar environment
- Each responsible for specific tasks
- Layers work together to facilitate data transmission
- Made up of seven distinct layers
	1. **Application**: HTTP, SSH, SMTP, DNS, etc
	2. **Presentation**: Compression and Encryption (SSL)
	3. **Session**: Management of connections (without error)
	4. **Transport**: TCP and UDP (assembling by sequence numbers)
	5. **Network**: IP address, routing (finding the best path to send data from the source to the destination by the router)
	6. **Data Link**: MAC address (physical address of computer)
	7. **Physical**: Physical layer

## Connection

- **IP**: internet address to reach a computer
- **Port**: various channels to communicate
- **Router**: routes the traffic to the destination
	- Making connection requires, IP and Port
	- IP: PORT → example: 192.168.1.2:25

![Untitled Diagram.drawio5.png](/images/Untitled_Diagram.drawio5.png)

### **TCP**

Responsible for:

- Breaking the data into manageable packets
- `Connection-oriented` protocol (res-sends of fails)
- Ensuring that packets reach their destination

### **UDP**

- Connectionless conversation

### 3-Way Handshake (TCP)

![Untitled Diagram.drawio6.png](/images/Untitled_Diagram.drawio6.png)

---

## DNS (Domain Name Service)

- Network Protocol
- Works over `UDP`
- We can send DNS queries using the `dig` and `ping` commands.
- Browser do this (`name resolution`)

```bash
dig memoryleaks.ir
ping memoryleaks.ir
```

![Untitled Diagram.drawio7.png](/images/Untitled_Diagram.drawio7.png)

- **DNS Query**: a request asking for the IP address
- **DNS Client**: a machine sending `name resolution` queries
	- **name resolution**: converting domain name into an IP address (domain → IP)
- **DNS Server**: a machine which responds to DNS queries
- **Name Server**: a machine which responds to DNS queries

---

### How and where does the DNS server IP address come from?

- The DNS server IP address is automatically assigned by the modem or the Internet Service Provider (ISP)
- Or it is manually entered by the user in the `/etc/resolv.conf` file

```bash
vim /etc/resolv.conf

nameserver 8.8.8.8
```

### Hierarchical process

![photo_2024-09-19_16-44-33.jpg](/images/photo_2024-09-19_16-44-33.jpg)

### Chain

- memoryleaks.ir → DNS → 127.0.0.1 → 192.168.1.1 → 8.8.8.8 → Root → .ir → NS

### Where are the name servers (NS) for a domain managed and configured?

- Domain Panel

---

## DNS Records

- **A** Record: IPv4 address
- **AAAA** Record: IPv6 address
- **CNAME** Record: pointing to another domain
- **MX** Record: mail servers responsible for receiving emails
- **TXT** Records: text information (anything)
- **NS** Record: authoritative name servers
	- **authoritative name servers**: legit name servers of a domain
    
	> We can manually configure the name servers or use the pre-configured name servers provided by service providers
	> 

```bash
dig A +short securityflow.io
dig AAAA +short securityflow.io
dig NS +short securityflow.io
dig CNAME +short securityflow.io
dig MX +short securityflow.io
dig TXT +short securityflow.io
```

## Host File

- Manual method of Name Resolution
- We perform that through the `/etc/hosts` file

```bash
vim /etc/hosts

127.0.0.1 attacker.com
```

---

## Web Server

- Serving resource to users by **HTTP** protocol (IP/TCP/HTTP)
- Different web servers, such as Apache and Nginx, have different configurations
- We can find the **main configuration** in `/etc/apache2/apache2.conf`
	- **directive**: a configuration command to change settings

### several sub-configuration files

- directive: **IncludeOptional** (IncludeOptional `another.conf`)

### Directives

- **DocumentRoot**:  directory where files and folders are (The default DocumentRoot path is set to `/var/www/html`)
- **ServerName**: hostname for a virtual host
- **Listen**: The IP and port where Apache is bound
- **ErrorLog**: location of the error files
- **Include**: including another configuration file
- **IncludeOptional**: including another configuration file
- **Directory**: to enclose a group of directories
- **Files**: To limit the scope of the enclosed directive
- **IfModule**: directive only applied if module is present

### Security

- Apache needs to start as root
- Listening socket on privileged ports
- Uses setuid to switch to user
	- **setuid**: execute a program with the permission of the program’s owner
	- **www-data** is the user

### Work Flow

![Untitled Diagram.drawio8.png](/images/Untitled_Diagram.drawio8.png)

### Virtual Host

- multiple hosts on a server
- Multiple domains can have the same A record and point to the same IP address. The server uses Virtual Host to determine which site to display for each domain

![Untitled Diagram.drawio9.png](/images/Untitled_Diagram.drawio9.png)

---

## Tips

1. DNS handles the domain
2. web-server handles the virtual host

---

## URL

- How to fetch a resource from a location

![Untitled Diagram.drawio10.png](/images/Untitled%20Diagram.drawio10.png)

## HTTP

- **request line**: Method SP Request-URI SP HTTP-Version CRLF
	- Method: http verb
		- to tell servers what action to perform on a resource (`GET`, `POST`, `HEAD`,`PUT`, `PATCH`, `DELETE`, `TRACE`, `OPTIONS`, etc)
	- SP: ASCII 32
	- Request-URI: /panel/user.php
	- HTTP-Version: HTTP/1.0
	- CRLF: ASCII 13 10
- **status line**: HTTP-Version SP Status-Code SP Reason-Phrase CRLF
	- Status-Code:
		- 200, success
		- 300, redirection
		- 400, client error
		- 500, server error
- **Headers**: field-name “:” [ field-value ] CRLF
	- additional information to request/response
	- Lists of name/value pairs
	- headers end by CRLF, then body comes
- **Body**:
	- contains the main data exchanged between the client and server, such as forms or webpage content

![Untitled Diagram.drawio44.png](/images/Untitled%20Diagram.drawio44.png)

## Headers

- **Host**: specifies the HOST of the server (virtual host)
- **Referer**: the address of the previous web page
- **User-Agent**: contains a characteristic string about client
- **Cookie**: contains stored HTTP cookies
- **Set-Cookie**: to set a cookie in the user’s web browser (Response Header)
- **Content-Length**: the size of the resource, in decimal number of bytes
- **Content-Type**: indicates the media type of resource
- **Location**: indicates the URL to redirect a page to

### Curl

- `curl` is a command-line tool for transferring data over network protocols like HTTP and FTP

```bash
curl -v https://35.163.77.62 #method: GET
curl -v https://35.163.77.62 -I #method: HEAD (doesn't have a response body)
curl -v https://35.163.77.62 -d "data=test" #method: POST
```

---

## CDN

- Is a reverse proxy
- distributes content

![Untitled Diagram.drawio666.png](/images/Untitled%20Diagram.drawio666.png)

### Proxy

- A proxy is an intermediary server that acts as a gateway between a client and another server, handling requests and responses on behalf of the client
- It is usually designed for users and clients, particularly for hiding user identity or filtering content

### Reverse Proxy

- A reverse proxy is a server that sits in front of one or more web servers, forwarding client requests to the appropriate backend server and returning the server's response to the client, effectively acting as an intermediary
- It is typically designed for servers and online services, especially for optimizing performance, security, and load balancing

### Upstream

- The origin IP that receives requests from a reverse proxy, serving as the source of content for client responses

### CDN Features

1. caching
2. load balancing
	- **Load balancing** means evenly distributing requests across upstream servers to improve speed and performance
3. global distribution
4. DDOS protection
5. security (waf)
	- If the request is not malicious, the CDN forwards it to the upstream server
6. stability

---

## Tasks

### **1. Name Server**

Ask the `A` record of `icollab.info` directly from its name servers by dig command

```bash
dig A icollab.info @ns1.icollab.info
```

### 2. **Reverse Proxy**

You should know reverse proxy concept and setup a simple reverse proxy by Nginx or Apache

- **Enable Necessary Modules:**
    
	```bash
	a2enmod proxy
	a2enmod proxy_http
	```
    
- **Configure the Reverse Proxy:**
Edit your Apache configuration file and add the following (`/etc/apache2/sites-available/000-default.conf`)
    
	```
	<VirtualHost *:80>
    
			ProxyPreserveHost On
			ProxyPass / http://127.0.0.1:3000/
			ProxyPassReverse / http://127.0.0.1:3000/
            
	</VirtualHost>
	```
    
- **Restart Apache**:
    
	```bash
	systemctl restart apache2
	```
# cURL Complete Cheatsheet

A clean and comprehensive **cURL command reference** suitable for GitHub documentation.

---

## 📌 Basic Usage

### **GET Request (default)**

```bash
curl https://example.com
```

### **Save Output to File**

```bash
curl https://example.com -o output.html
```

### **Follow Redirects**

```bash
curl -L https://example.com
```

### **Silent Mode (no progress bar)**

```bash
curl -s https://example.com
```

---

## 📌 Request Details

### **Verbose Output (show full request/response)**

```bash
curl -v https://example.com
```

### **Show Only Headers (HEAD)**

```bash
curl -I https://example.com
```

### **Show Only Response Headers**

```bash
curl -s -D - https://example.com -o /dev/null
```

### **Show Only Status Code**

```bash
curl -o /dev/null -s -w "%{http_code}\n" https://example.com
```

---

## 📌 HTTP Methods

### **POST**

```bash
curl -X POST https://example.com -d "name=John&age=30"
```

### **PUT**

```bash
curl -X PUT https://example.com/users/1 -d "name=Mike"
```

### **PATCH**

```bash
curl -X PATCH https://example.com/users/1 -d "status=active"
```

### **DELETE**

```bash
curl -X DELETE https://example.com/users/1
```

### **Custom Method**

```bash
curl -X OPTIONS https://example.com
```

---

## 📌 Sending Data

### **Form Data (application/x-www-form-urlencoded)**

```bash
curl -d "user=test&pass=123" https://example.com/login
```

### **JSON Body**

```bash
curl -X POST https://example.com/api \
     -H "Content-Type: application/json" \
     -d '{"key":"value"}'
```

### **Send File as Data**

```bash
curl -X POST -d @data.json https://example.com/api
```

---

## 📌 Headers

### **Add a Header**

```bash
curl -H "User-Agent: CustomAgent" https://example.com
```

### **Multiple Headers**

```bash
curl -H "Accept: application/json" -H "X-Test: true" https://example.com
```

### **Send Cookies**

```bash
curl -H "Cookie: session=abcd1234" https://example.com
```

### **Store Cookies**

```bash
curl -c cookies.txt https://example.com
```

### **Send Cookies from File**

```bash
curl -b cookies.txt https://example.com
```

---

## 📌 Authentication

### **Basic Authentication**

```bash
curl -u username:password https://example.com
```

### **Bearer Token**

```bash
curl -H "Authorization: Bearer TOKEN" https://example.com
```

### **Client Certificate**

```bash
curl --cert mycert.pem https://example.com
```

---

## 📌 Network Options

### **Specify Interface**

```bash
curl --interface eth0 https://example.com
```

### **Set Timeout**

```bash
curl --max-time 10 https://example.com
```

### **Use Proxy**

```bash
curl -x http://127.0.0.1:8080 https://example.com
```

### **SOCKS Proxy**

```bash
curl -x socks5://127.0.0.1:9050 https://example.com
```

---

## 📌 File Transfer

### **Download a File**

```bash
curl -O https://example.com/file.zip
```

### **Upload a File**

```bash
curl -T upload.zip https://example.com/upload/
```

### **Multipart Upload**

```bash
curl -F "file=@image.png" https://example.com/upload
```

---

## 📌 Debugging & Testing

### **Show DNS Details**

```bash
curl -v --trace-ascii debug.txt https://example.com
```

### **Test HTTPS Certificate**

```bash
curl -vI https://example.com
```

### **Ignore Certificate Verification**

```bash
curl -k https://example.com
```

---

## 📌 Output Formatting

### **Show Total Time**

```bash
curl -w "%{time_total}\n" -o /dev/null -s https://example.com
```

### **Show Full Performance Stats**

```bash
curl -w "\nTime Connect: %{time_connect}\nTime Total: %{time_total}\nSpeed: %{speed_download}\n" \
     -o /dev/null -s https://example.com
```

---

## 📌 FTP / SFTP

### **Download via FTP**

```bash
curl ftp://example.com/file.zip -u user:pass -O
```

### **Upload via FTP**

```bash
curl -T file.zip ftp://example.com/ -u user:pass
```

### **SFTP Upload**

```bash
curl -T file.txt sftp://example.com/home/user/ -u user:pass
```

---

## 📌 Useful Combined Examples

### **Send JSON + Custom Header + Save Output**

```bash
curl -X POST https://example.com/api \
     -H "Content-Type: application/json" \
     -H "X-API-Key: 123" \
     -d '{"test":"ok"}' \
     -o response.json
```

### **Follow Redirects + Save Cookies + Verbose**

```bash
curl -L -c cookies.txt -v https://example.com
```

---

### ✔ This cheatsheet is ready for GitHub.

If you want, I can also:

* Add icons / emoji versions
* Create an advanced cURL guide
* Add examples for API pentesting
* Make a downloadable PDF version

# **`dig` Command – Complete Cheatsheet**

A clear, GitHub-ready reference for using **`dig` (Domain Information Groper)** — a DNS lookup tool for querying DNS records.

---

# 📌 **Basic Syntax**

```
dig [options] [domain] [record_type] [@name_server]
```

---

# 🚀 **Most Common Commands**

## **1. Simple A Record Lookup**

```
dig example.com
```

## **2. Query Specific DNS Record**

```
dig A example.com
```

Examples:

```
dig A example.com

dig AAAA example.com

dig MX example.com

dig NS example.com

dig TXT example.com

dig CNAME www.example.com
```

---

# 🎯 **Short Output (clean result only)**

```
dig +short example.com

dig A +short example.com

dig MX +short example.com

dig TXT +short example.com
```

---

# 🧭 **Querying a Specific Name Server**

Use `@nameserver` to ask a domain's authoritative server:

```
dig A example.com @ns1.example.com
```

---

# 🔍 **Reverse DNS Lookup**

```
dig -x 8.8.8.8
```

---

# 🛠️ **Using Flags / Options**

## **1. +trace** (Full DNS resolution path)

```
dig example.com +trace
```

Shows: Root → TLD → Authoritative NS

## **2. +nocmd** (Hide the header)

```
dig example.com +nocmd
```

## **3. +noall +answer** (Only show the answer section)

```
dig example.com +noall +answer
```

## **4. +stats** (Show statistics)

```
dig example.com +stats
```

## **5. Ask for ANY record**

```
dig ANY example.com
```

*(Not all servers respond to ANY queries anymore)*

---

# 🌍 **DNS Server Info**

## **Check which DNS server answered:**

```
dig example.com | grep SERVER
```

## **Specify DNS port**

```
dig @8.8.8.8 -p 53 example.com
```

---

# 🧪 **SOA Record (Start of Authority)**

```
dig SOA example.com
```

---

# 🧵 **Query Multiple Records at Once**

```
dig example.com A MX TXT
```

---

# 🔄 **Query DNS over TCP**

```
dig +tcp example.com
```

---

# 📡 **Query with Custom Resolver File**

```
dig -f domains.txt
```

Where `domains.txt` contains a list of domains.

---

# 📁 **Output to File**

```
dig example.com > output.txt
```

---

# 🔐 **DNSSEC Checks**

```
dig example.com +dnssec
```

---

# 🧩 **Full Example Session**

```
dig A example.com @8.8.8.8 +trace +stats
```

---

# 🏁 **Quick Reference Table**

| Task                | Command                          |
| ------------------- | -------------------------------- |
| Get A record        | `dig A domain.com`               |
| Short IP only       | `dig +short domain.com`          |
| Get MX records      | `dig MX domain.com`              |
| Query NS            | `dig NS domain.com`              |
| Reverse DNS         | `dig -x IP`                      |
| Ask specific NS     | `dig domain.com @ns1.domain.com` |
| Trace resolution    | `dig +trace domain.com`          |
| Only answer section | `dig +noall +answer domain.com`  |
| DNSSEC check        | `dig +dnssec domain.com`         |

---
# **`dig` Command – Complete Cheatsheet**

A clear, GitHub-ready reference for using **`dig` (Domain Information Groper)** — a DNS lookup tool for querying DNS records.

---

# 📌 **Basic Syntax**

```
dig [options] [domain] [record_type] [@name_server]
```

---

# 🚀 **Most Common Commands**

## **1. Simple A Record Lookup**

```
dig example.com
```

## **2. Query Specific DNS Record**

```
dig A example.com
```

Examples:

```
dig A example.com

dig AAAA example.com

dig MX example.com

dig NS example.com

dig TXT example.com

dig CNAME www.example.com
```

---

# 🎯 **Short Output (clean result only)**

```
dig +short example.com

dig A +short example.com

dig MX +short example.com

dig TXT +short example.com
```

---

# 🧭 **Querying a Specific Name Server**

Use `@nameserver` to ask a domain's authoritative server:

```
dig A example.com @ns1.example.com
```

---

# 🔍 **Reverse DNS Lookup**

```
dig -x 8.8.8.8
```

---

# 🛠️ **Using Flags / Options**

## **1. +trace** (Full DNS resolution path)

```
dig example.com +trace
```

Shows: Root → TLD → Authoritative NS

## **2. +nocmd** (Hide the header)

```
dig example.com +nocmd
```

## **3. +noall +answer** (Only show the answer section)

```
dig example.com +noall +answer
```

## **4. +stats** (Show statistics)

```
dig example.com +stats
```

## **5. Ask for ANY record**

```
dig ANY example.com
```

*(Not all servers respond to ANY queries anymore)*

---

# 🌍 **DNS Server Info**

## **Check which DNS server answered:**

```
dig example.com | grep SERVER
```

## **Specify DNS port**

```
dig @8.8.8.8 -p 53 example.com
```

---

# 🧪 **SOA Record (Start of Authority)**

```
dig SOA example.com
```

---

# 🧵 **Query Multiple Records at Once**

```
dig example.com A MX TXT
```

---

# 🔄 **Query DNS over TCP**

```
dig +tcp example.com
```

---

# 📡 **Query with Custom Resolver File**

```
dig -f domains.txt
```

Where `domains.txt` contains a list of domains.

---

# 📁 **Output to File**

```
dig example.com > output.txt
```

---

# 🔐 **DNSSEC Checks**

```
dig example.com +dnssec
```

---

# 🧩 **Full Example Session**

```
dig A example.com @8.8.8.8 +trace +stats
```

---

# 🏁 **Quick Reference Table**

| Task                | Command                          |
| ------------------- | -------------------------------- |
| Get A record        | `dig A domain.com`               |
| Short IP only       | `dig +short domain.com`          |
| Get MX records      | `dig MX domain.com`              |
| Query NS            | `dig NS domain.com`              |
| Reverse DNS         | `dig -x IP`                      |
| Ask specific NS     | `dig domain.com @ns1.domain.com` |
| Trace resolution    | `dig +trace domain.com`          |
| Only answer section | `dig +noall +answer domain.com`  |
| DNSSEC check        | `dig +dnssec domain.com`         |

---
