# TCP/IP Fundamentals

Understanding TCP/IP networking - the foundation for all network security and penetration testing.

---

## 📚 Table of Contents

- [What is TCP/IP?](#what-is-tcpip)
- [The OSI Model](#the-osi-model)
- [IP Addressing](#ip-addressing)
- [Subnetting](#subnetting)
- [TCP vs UDP](#tcp-vs-udp)
- [Common Ports](#common-ports)
- [DNS Basics](#dns-basics)
- [Network Tools](#network-tools)

---

## What is TCP/IP?

**TCP/IP** (Transmission Control Protocol/Internet Protocol) is the fundamental communication protocol suite that powers the internet and most networks.

**Key Components:**
- **IP (Internet Protocol)** - Addressing and routing packets
- **TCP (Transmission Control Protocol)** - Reliable, connection-oriented communication
- **UDP (User Datagram Protocol)** - Fast, connectionless communication

---

## The OSI Model

The **OSI (Open Systems Interconnection) Model** has 7 layers:

| Layer | Name | Function | Examples |
|-------|------|----------|----------|
| 7 | Application | User applications | HTTP, FTP, SSH, DNS |
| 6 | Presentation | Data formatting | SSL/TLS, JPEG, ASCII |
| 5 | Session | Manages connections | NetBIOS, RPC |
| 4 | Transport | End-to-end delivery | TCP, UDP |
| 3 | Network | Routing packets | IP, ICMP, OSPF |
| 2 | Data Link | Node-to-node transfer | Ethernet, Wi-Fi, ARP |
| 1 | Physical | Physical transmission | Cables, Wi-Fi signals |

**Mnemonic:** "**P**lease **D**o **N**ot **T**hrow **S**ausage **P**izza **A**way"

### TCP/IP Model (Simplified)

The TCP/IP model combines OSI layers into 4:

| TCP/IP Layer | OSI Equivalent | Examples |
|--------------|----------------|----------|
| Application | 5-7 | HTTP, DNS, FTP |
| Transport | 4 | TCP, UDP |
| Internet | 3 | IP, ICMP |
| Network Access | 1-2 | Ethernet, Wi-Fi |

---

## IP Addressing

### IPv4 Addresses

**IPv4** uses 32-bit addresses written as 4 octets:

```
192.168.1.100
```

Each octet: 0-255 (8 bits)

**Classes:**

| Class | Range | Default Mask | Usage |
|-------|-------|--------------|-------|
| A | 1.0.0.0 - 126.255.255.255 | 255.0.0.0 (/8) | Large networks |
| B | 128.0.0.0 - 191.255.255.255 | 255.255.0.0 (/16) | Medium networks |
| C | 192.0.0.0 - 223.255.255.255 | 255.255.255.0 (/24) | Small networks |
| D | 224.0.0.0 - 239.255.255.255 | N/A | Multicast |
| E | 240.0.0.0 - 255.255.255.255 | N/A | Reserved |

### Private IP Ranges

**RFC 1918** defines private (non-routable) IP ranges:

```
10.0.0.0        - 10.255.255.255   (10.0.0.0/8)
172.16.0.0      - 172.31.255.255   (172.16.0.0/12)
192.168.0.0     - 192.168.255.255  (192.168.0.0/16)
```

These are used for local networks (home, office) and not routed on the internet.

### Special IP Addresses

```
127.0.0.1       - Localhost (loopback)
0.0.0.0         - Default route / Any address
255.255.255.255 - Broadcast address
169.254.x.x     - APIPA (Auto-assigned when DHCP fails)
```

### IPv6 Addresses

**IPv6** uses 128-bit addresses:

```
2001:0db8:85a3:0000:0000:8a2e:0370:7334

# Shortened:
2001:db8:85a3::8a2e:370:7334
```

**Why IPv6?**
- IPv4: ~4.3 billion addresses (running out!)
- IPv6: ~340 undecillion addresses (practically unlimited)

---

## Subnetting

**Subnetting** divides large networks into smaller subnetworks.

### Subnet Masks

A subnet mask defines which part is network vs host:

```
IP:     192.168.1.100
Mask:   255.255.255.0
        Network: 192.168.1.0
        Host: 0.0.0.100
```

### CIDR Notation

**CIDR** (Classless Inter-Domain Routing) is a shorthand:

```
192.168.1.0/24
```

The `/24` means first 24 bits are network (8 bits for hosts).

**Common Notations:**

| CIDR | Subnet Mask | Hosts | Usage |
|------|-------------|-------|-------|
| /32 | 255.255.255.255 | 1 | Single host |
| /24 | 255.255.255.0 | 254 | Small network |
| /16 | 255.255.0.0 | 65,534 | Medium network |
| /8 | 255.0.0.0 | 16,777,214 | Large network |

### Calculating Hosts

Formula: `2^(32 - prefix) - 2`

```
/24 network:
2^(32-24) - 2 = 2^8 - 2 = 256 - 2 = 254 usable hosts

Why -2?
- Network address (192.168.1.0)
- Broadcast address (192.168.1.255)
```

### Subnet Examples

**Example 1: Home Network**
```
Network: 192.168.1.0/24
Range: 192.168.1.1 - 192.168.1.254
Broadcast: 192.168.1.255
Typical Router: 192.168.1.1
```

**Example 2: Smaller Subnet**
```
Network: 192.168.1.0/28
Usable: 2^4 - 2 = 14 hosts
Range: 192.168.1.1 - 192.168.1.14
Broadcast: 192.168.1.15
```

---

## TCP vs UDP

### TCP (Transmission Control Protocol)

**Reliable, connection-oriented** protocol:

✅ **Features:**
- Guaranteed delivery
- Ordered packets
- Error checking
- Flow control
- 3-way handshake

**TCP 3-Way Handshake:**
```
Client → Server: SYN (Synchronize)
Server → Client: SYN-ACK (Synchronize-Acknowledge)
Client → Server: ACK (Acknowledge)
[Connection established]
```

**Use Cases:**
- Web browsing (HTTP/HTTPS)
- Email (SMTP, IMAP)
- File transfers (FTP, SFTP)
- SSH

### UDP (User Datagram Protocol)

**Fast, connectionless** protocol:

⚡ **Features:**
- No connection setup
- No delivery guarantee
- No packet ordering
- Lower overhead
- Faster than TCP

**Use Cases:**
- DNS queries
- Video/audio streaming
- Online gaming
- VoIP calls
- DHCP

### Comparison

| Feature | TCP | UDP |
|---------|-----|-----|
| Reliability | ✅ Guaranteed | ❌ Best effort |
| Speed | Slower | ⚡ Faster |
| Ordering | ✅ Ordered | ❌ Unordered |
| Overhead | Higher | Lower |
| Use Case | Accuracy matters | Speed matters |

---

## Common Ports

### Well-Known Ports (0-1023)

| Port | Protocol | Service | Description |
|------|----------|---------|-------------|
| 20/21 | TCP | FTP | File Transfer |
| 22 | TCP | SSH | Secure Shell |
| 23 | TCP | Telnet | Unencrypted remote access |
| 25 | TCP | SMTP | Email sending |
| 53 | TCP/UDP | DNS | Domain Name System |
| 67/68 | UDP | DHCP | IP address assignment |
| 80 | TCP | HTTP | Web traffic |
| 110 | TCP | POP3 | Email retrieval |
| 143 | TCP | IMAP | Email retrieval |
| 443 | TCP | HTTPS | Secure web traffic |
| 445 | TCP | SMB | Windows file sharing |
| 3389 | TCP | RDP | Remote Desktop |

### Registered Ports (1024-49151)

| Port | Protocol | Service |
|------|----------|---------|
| 3306 | TCP | MySQL |
| 5432 | TCP | PostgreSQL |
| 5900 | TCP | VNC |
| 8080 | TCP | HTTP alternate |
| 8443 | TCP | HTTPS alternate |

### Dynamic Ports (49152-65535)

Used for temporary client connections.

---

## DNS Basics

**DNS** (Domain Name System) translates names to IP addresses.

### DNS Hierarchy

```
.                    (Root)
  └── .com           (Top-Level Domain - TLD)
        └── example.com   (Domain)
              └── www.example.com  (Subdomain)
```

### DNS Record Types

| Record | Purpose | Example |
|--------|---------|---------|
| A | IPv4 address | example.com → 192.0.2.10 |
| AAAA | IPv6 address | example.com → 2001:db8::1 |
| CNAME | Canonical name (alias) | www → example.com |
| MX | Mail server | mail.example.com |
| NS | Name server | ns1.example.com |
| TXT | Text information | Verification, SPF |

### DNS Lookup Commands

```bash
# Basic DNS lookup
nslookup example.com

# Detailed DNS info
dig example.com

# Reverse lookup (IP to hostname)
dig -x 192.0.2.10

# Query specific record type
dig example.com MX
dig example.com TXT
```

**Example Output:**
```bash
$ dig example.com A

;; ANSWER SECTION:
example.com.  300  IN  A  192.0.2.10
```

---

## Network Tools

### ping

Test connectivity to a host:

```bash
# Ping IP address
ping 192.0.2.10

# Ping hostname
ping example.com

# Send 4 packets and stop
ping -c 4 example.com

# Continuous ping (Ctrl+C to stop)
ping example.com
```

**What ping does:**
- Sends ICMP Echo Request
- Waits for ICMP Echo Reply
- Measures round-trip time (RTT)

### traceroute / tracert

Trace the path packets take:

```bash
# Linux/Mac
traceroute example.com

# Windows
tracert example.com
```

Shows each "hop" (router) between you and destination.

### netstat

Display network connections:

```bash
# Show all connections
netstat -a

# Show listening ports
netstat -l

# Show with program names (Linux)
netstat -tulpn

# Show TCP connections only
netstat -t
```

### ip / ifconfig

View network configuration:

```bash
# Modern Linux (ip command)
ip addr show
ip route show

# Legacy (ifconfig)
ifconfig

# Show routing table
ip route
route -n
```

### ARP

View ARP cache (IP ↔ MAC mapping):

```bash
# Show ARP table
arp -a

# Clear ARP cache (Linux)
ip -s -s neigh flush all

# Add static ARP entry
arp -s 192.0.2.10 aa:bb:cc:dd:ee:ff
```

---

## Practical Examples

### Example 1: Home Network

```
Internet
   |
[Router: 192.168.1.1]
   |
   ├── Computer 1: 192.168.1.100
   ├── Computer 2: 192.168.1.101
   ├── Phone: 192.168.1.102
   └── IoT Device: 192.168.1.103

Network: 192.168.1.0/24
Gateway: 192.168.1.1
DNS: 8.8.8.8 (Google Public DNS)
```

### Example 2: Office Network with Subnets

```
Network: 10.0.0.0/16

Subnet 1 (Admin):     10.0.1.0/24
Subnet 2 (Sales):     10.0.2.0/24
Subnet 3 (IT):        10.0.3.0/24
Subnet 4 (Guest):     10.0.100.0/24
```

### Example 3: Testing Connectivity

```bash
# Step 1: Check local IP
ip addr show

# Step 2: Ping gateway
ping 192.168.1.1

# Step 3: Ping external IP
ping 8.8.8.8

# Step 4: Test DNS
ping google.com

# If Step 3 works but Step 4 fails → DNS issue
# If Step 2 fails → Local network issue
# If all fail → Network adapter issue
```

---

## Security Considerations

### Network Scanning Detection

Defenders look for:
- Port scan patterns (sequential ports)
- SYN floods (many half-open connections)
- Unusual traffic volumes
- Connections to unexpected ports

### Firewalls

**Firewall Rules** control traffic:

```bash
# Allow SSH from specific IP
iptables -A INPUT -p tcp --dport 22 -s 192.0.2.50 -j ACCEPT

# Block all other SSH
iptables -A INPUT -p tcp --dport 22 -j DROP

# Allow established connections
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
```

### NAT (Network Address Translation)

**NAT** allows multiple devices to share one public IP:

```
Private Network (192.168.1.0/24)
    ↓
[Router with NAT]
    ↓
Public IP (203.0.113.50)
    ↓
Internet
```

---

## Practice Exercises

1. Calculate usable hosts for /25, /26, /27 subnets
2. Identify your network's IP range and gateway
3. Use `traceroute` to see path to google.com
4. Find which process is listening on port 80
5. Perform DNS lookup for 10 different domains

---

## Quick Reference

### Subnet Mask Cheat Sheet

```
/32 = 255.255.255.255 (1 host)
/31 = 255.255.255.254 (2 hosts)
/30 = 255.255.255.252 (4 hosts)
/29 = 255.255.255.248 (8 hosts)
/28 = 255.255.255.240 (16 hosts)
/27 = 255.255.255.224 (32 hosts)
/26 = 255.255.255.192 (64 hosts)
/25 = 255.255.255.128 (128 hosts)
/24 = 255.255.255.0 (256 hosts)
/16 = 255.255.0.0 (65,536 hosts)
/8  = 255.0.0.0 (16,777,216 hosts)
```

### Essential Commands

```bash
# IP configuration
ip addr show          # Linux
ipconfig             # Windows

# Test connectivity
ping hostname
traceroute hostname

# DNS lookup
dig hostname
nslookup hostname

# View connections
netstat -tulpn       # Linux
netstat -ano         # Windows

# View routing
ip route             # Linux
route print          # Windows
```

---

## See Also

- [Network Services](network-services.md) - Services that use TCP/IP
- [NMAP Scanning](nmap-scanning.md) - Scan TCP/IP networks
- [Wireshark Basics](wireshark-basics.md) - Analyze TCP/IP packets
