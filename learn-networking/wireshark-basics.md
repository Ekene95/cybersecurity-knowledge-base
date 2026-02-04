# Wireshark Basics

Master packet analysis with Wireshark - see exactly what's happening on your network.

---

## 📚 Table of Contents

- [What is Wireshark?](#what-is-wireshark)
- [Installation & Setup](#installation--setup)
- [Capturing Packets](#capturing-packets)
- [Display Filters](#display-filters)
- [Analyzing Protocols](#analyzing-protocols)
- [Following Streams](#following-streams)
- [Common Use Cases](#common-use-cases)
- [Security Analysis](#security-analysis)

---

## What is Wireshark?

**Wireshark** is a free, open-source network protocol analyzer that lets you capture and analyze network traffic in real-time.

**Use Cases:**
- 🔍 Troubleshoot network problems
- 🔐 Analyze security issues
- 📊 Understand network protocols
- 🐛 Debug network applications
- 🎓 Learn how networks work

**⚠️ Legal Warning:** Only capture traffic on networks you own or have permission to monitor!

---

## Installation & Setup

### Linux (Debian/Ubuntu)

```bash
# Install Wireshark
sudo apt update
sudo apt install wireshark

# Add your user to wireshark group (capture without sudo)
sudo usermod -aG wireshark $USER

# Log out and back in for changes to take effect
```

### macOS

```bash
# Using Homebrew
brew install --cask wireshark
```

### Windows

Download from: https://www.wireshark.org/download.html

---

## Capturing Packets

### Start Capture

1. **Launch Wireshark**
2. **Select Interface** (eth0, wlan0, etc.)
3. **Click Start** (shark fin icon)

### Command Line (tshark)

```bash
# Capture on interface eth0
sudo tshark -i eth0

# Capture and save to file
sudo tshark -i eth0 -w capture.pcap

# Capture for 60 seconds
sudo tshark -i eth0 -a duration:60 -w capture.pcap
```

### Capture Filters

Set **before** starting capture:

```
# Capture only HTTP traffic
port 80

# Capture specific host
host 192.0.2.10

# Capture entire subnet
net 192.0.2.0/24

# Capture TCP traffic
tcp

# Capture SSH
port 22
```

---

## Display Filters

**Display filters** filter already-captured packets.

### Basic Filters

```
# Filter by Protocol
http
https
ssh
ftp
dns
tcp
udp
icmp

# Filter by IP address
ip.addr == 192.0.2.10        # Either source or destination
ip.src == 192.0.2.10         # Source IP
ip.dst == 192.0.2.10         # Destination IP

# Filter by Port
tcp.port == 80               # TCP port 80
tcp.port == 443              # HTTPS
udp.port == 53               # DNS
```

### HTTP Filters

```
# All HTTP traffic
http

# HTTP GET requests
http.request.method == "GET"

# HTTP POST requests
http.request.method == "POST"

# Specific hostname
http.host == "example.com"

# HTTP responses
http.response

# HTTP 200 OK
http.response.code == 200

# HTTP errors
http.response.code >= 400
```

### Advanced Filters

```
# Exclude specific IP
!(ip.addr == 192.0.2.10)
not ip.addr == 192.0.2.10

# Multiple conditions (AND)
ip.src == 192.0.2.10 and tcp.port == 80

# Multiple conditions (OR)
tcp.port == 80 or tcp.port == 443

# Contains string
http contains "password"
```

### Useful Filter Examples

```
# Show only SYN packets (TCP handshake)
tcp.flags.syn == 1 and tcp.flags.ack == 0

# Show only ACK packets
tcp.flags.ack == 1

# Show TCP retransmissions (problems!)
tcp.analysis.retransmission

# Show DNS queries
dns.flags.response == 0

# Show DNS responses
dns.flags.response == 1

# Show ARP traffic
arp
```

---

## Analyzing Protocols

### TCP Stream Analysis

**TCP 3-Way Handshake:**

```
Client → Server: [SYN]
Server → Client: [SYN, ACK]
Client → Server: [ACK]
[Connection established]
```

**In Wireshark:**
- Look for Flags: `[SYN]`, `[SYN, ACK]`, `[ACK]`
- Check sequence numbers
- Verify handshake completion

### HTTP Analysis

```
# Filter HTTP
http

# Find GET requests
http.request.method == "GET"

# Check User-Agent headers
http.user_agent

# Find cookies
http.cookie
```

**Example HTTP Request:**
```
GET /index.html HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
```

### DNS Analysis

```
# All DNS traffic
dns

# DNS queries
dns.qry.name

# Find lookups for specific domain
dns.qry.name contains "example"
```

---

## Following Streams

**Follow TCP Stream** shows entire conversation between client and server.

### How to Follow Stream

1. **Right-click** on any packet
2. Select **Follow → TCP Stream**
3. See full conversation in new window

**Keyboard Shortcut:** `Ctrl+Alt+Shift+T`

### Follow HTTP Stream

```
1. Filter: http
2. Right-click on HTTP packet
3. Follow → TCP Stream
4. See full HTTP request/response
```

**Example Output:**
```
GET /login HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0

HTTP/1.1 200 OK
Content-Type: text/html
Set-Cookie: session=abc123

<!DOCTYPE html>
<html>...
```

### Stream Index

Each stream has an index number:

```
# View specific stream
tcp.stream eq 0
tcp.stream eq 1
tcp.stream eq 2
```

---

## Common Use Cases

### 1. Troubleshoot Slow Website

```bash
# Filter for specific website
http.host == "example.com"

# Check for:
# - Long response times
# - Retransmissions (tcp.analysis.retransmission)
# - Large response sizes
```

### 2. Find Unencrypted Passwords

```bash
# Search for "password" in HTTP
http contains "password"

# Search in any protocol
frame contains "password"

# This is why HTTPS is important!
```

### 3. Analyze DNS Queries

```bash
# See all DNS queries
dns.qry.name

# Find queries to specific domain
dns.qry.name contains "google"

# Check DNS response times
dns.time
```

### 4. Detect Port Scans

```bash
# Look for many SYN packets
tcp.flags.syn == 1 and tcp.flags.ack == 0

# Group by destination
# Many SYN to different ports = port scan!
```

### 5. Find FTP Credentials

```bash
# Filter FTP
ftp

# Look for USER and PASS commands
ftp.request.command == "USER"
ftp.request.command == "PASS"

# FTP sends passwords in cleartext!
```

---

## Security Analysis

### Detecting Attacks

**1. SYN Flood (DoS Attack)**
```
tcp.flags.syn == 1 and tcp.flags.ack == 0

# Many SYN packets without completion
# Indicates possible SYN flood attack
```

**2. ARP Spoofing**
```
arp.duplicate-address-detected

# Multiple MAC addresses for same IP
# Possible ARP poisoning/MitM attack
```

**3. DNS Tunneling**
```
dns and frame.len > 100

# Unusually large DNS queries
# May indicate data exfiltration
```

### Finding Credentials

**Warning:** Only analyze traffic you're authorized to capture!

```bash
# HTTP Basic Auth (base64)
http.authbasic

# FTP login
ftp.request.command == "USER" or ftp.request.command == "PASS"

# SMTP auth
smtp.auth

# Unencrypted protocols sending passwords
frame contains "password"
```

---

## Export Objects

Extract files from captured traffic:

1. **File → Export Objects → HTTP**
2. See all HTTP transferred files
3. **Save** interesting files

**Supports:**
- HTTP objects
- SMB files
- FTP transfers
- TFTP  transfers

---

## Statistics & Analysis

### Protocol Hierarchy

**Statistics → Protocol Hierarchy**

Shows breakdown of protocols in capture:
```
Frame: 1000 packets
  Ethernet: 1000 packets
    IP: 950 packets
      TCP: 800 packets
        HTTP: 500 packets
        HTTPS: 300 packets
      UDP: 150 packets
        DNS: 150 packets
```

### Conversations

**Statistics → Conversations**

Shows all conversations between hosts:
- IP conversations
- TCP conversations
- UDP conversations

### Endpoints

**Statistics → Endpoints**

Shows all communicating endpoints with:
- Packets sent/received
- Bytes transferred
- Top talkers

---

## Wireshark Tips & Tricks

### Colorization

Wireshark colors packets by type:
- **Light purple** - TCP
- **Light blue** - UDP
- **Black** - Errors/problems
- **Green** - HTTP
- **Light green** - DNS

### Time Display

**View → Time Display Format**
- Seconds since beginning of capture
- Time of day
- Seconds since previous packet

### Packet Bytes Pane

Bottom pane shows:
- Hex dump of packet
- ASCII representation
- Click field to highlight bytes

### Save Displayed Packets

```
File → Export Specified Packets
Select "Displayed" to save only filtered packets
```

---

## Command Line (tshark)

**tshark** is command-line Wireshark:

```bash
# List interfaces
tshark -D

# Capture on eth0
sudo tshark -i eth0

# Apply display filter
tshark -i eth0 -Y "http"

# Read from file
tshark -r capture.pcap

# Extract HTTP hosts
tshark -r capture.pcap -Y http -T fields -e http.host

# Count packets by protocol
tshark -r capture.pcap -q -z io,phs
```

---

## Practice Exercises

1. Capture your web browsing and find HTTP User-Agent
2. Follow a TCP stream for an HTTP connection
3. Find all DNS queries in a capture
4. Identify the three TCP handshake packets
5. Export images from HTTP traffic

---

## Quick Reference

### Essential Display Filters

```
# Protocols
http
https
dns
ssh
ftp

# IP Filters
ip.addr == 192.0.2.10
ip.src == 192.0.2.10
ip.dst == 192.0.2.10

# Port Filters
tcp.port == 80
tcp.port == 443
udp.port == 53

# Combined
ip.src == 192.0.2.10 and tcp.port == 80

# Exclude
!(ip.addr == 192.0.2.10)

# Contains
http contains "password"
```

### Keyboard Shortcuts

```
Ctrl+E    - Start/stop capture
Ctrl+K    - Capture options
Ctrl+F    - Find packet
Ctrl+G    - Go to packet
Ctrl+Alt+Shift+T - Follow TCP stream
```

---

## See Also

- [TCP/IP Fundamentals](tcpip-fundamentals.md) - Understand what you're capturing
- [NMAP Scanning](nmap-scanning.md) - Generate traffic to analyze
- [Network Services](network-services.md) - Services you'll see in captures
