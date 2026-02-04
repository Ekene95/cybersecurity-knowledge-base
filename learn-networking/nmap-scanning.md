# NMAP Scanning

Master network scanning with NMAP - the industry-standard tool for network discovery and security auditing.

---

## 📚 Table of Contents

- [What is NMAP?](#what-is-nmap)
- [Basic Scanning](#basic-scanning)
- [Scan Types](#scan-types)
- [Advanced Options](#advanced-options)
- [Output Formats](#output-formats)
- [Practical Examples](#practical-examples)
- [Evasion Techniques](#evasion-techniques)

---

## What is NMAP?

**NMAP** (Network Mapper) is a free, open-source tool for:
- 🔍 Network discovery
- 🔐 Security auditing  
- 🖥️ OS detection
- 🔌 Port scanning
- 📡 Service enumeration

**⚠️ Legal Warning:** Only scan networks you own or have written permission to test!

---

## Basic Scanning

### Simple Host Scan

```bash
# Scan single host
nmap 192.0.2.10

# Scan hostname
nmap scanme.nmap.org

# Scan multiple hosts
nmap 192.0.2.10 192.0.2.11 192.0.2.12
```

### Subnet Scanning

```bash
# Scan entire subnet (/24 network = 256 IPs)
nmap 192.0.2.0/24

# Alternative notations
nmap 192.0.2.*
nmap 192.0.2.0-255

# Range of IPs
nmap 192.0.2.10-50
```

---

## Scan Types

### TCP SYN Scan (Default)

**Fastest and stealthiest** - doesn't complete TCP handshake

```bash
# SYN scan (requires root)
sudo nmap -sS 192.0.2.10

# Half-open scan - doesn't complete connection
# Faster and less likely to be logged
```

### TCP Connect Scan

**Full connection** - completes TCP 3-way handshake

```bash
# Connect scan (doesn't require root)
nmap -sT 192.0.2.10

# Slower but works without privileges
# Connections will be logged by target
```

### UDP Scan

Scan UDP ports (slower than TCP):

```bash
# UDP scan
sudo nmap -sU 192.0.2.10

# Scan specific UDP ports
sudo nmap -sU -p 53,161,162 192.0.2.10
```

### Ping Scan (Host Discovery)

Check which hosts are online:

```bash
# Ping scan only (no port scan)
nmap -sn 192.0.2.0/24

# Also called "ping sweep"
# Useful for finding alive hosts quickly
```

---

## Advanced Options

### Aggressive Scan (-A)

**All-in-one scan** - combines 4 features:

```bash
nmap -A 192.0.2.10

# Enables:
# 1. OS detection (-O)
# 2. Version detection (-sV)
# 3. Script scanning (-sC)
# 4. Traceroute (--traceroute)
```

### OS Detection

```bash
# Detect operating system
sudo nmap -O 192.0.2.10

# Example output:
# Running:  Linux 4.X|5.X
# OS details: Linux 4.15 - 5.6
```

### Service/Version Detection

```bash
# Detect service versions
nmap -sV 192.0.2.10

# Example output:
# 22/tcp  open  ssh      OpenSSH 8.2p1
# 80/tcp  open  http     nginx 1.18.0
# 443/tcp open  https    nginx 1.18.0
```

### Port Specification

```bash
# Scan specific ports
nmap -p 22,80,443 192.0.2.10

# Scan port range
nmap -p 1-1000 192.0.2.10

# Scan all ports (1-65535)
nmap -p- 192.0.2.10

# Scan top 100 most common ports
nmap --top-ports 100 192.0.2.10
```

---

## Speed & Performance

### Timing Templates

NMAP offers 6 timing templates:

```bash
# T0 - Paranoid (slowest, IDS evasion)
nmap -T0 192.0.2.10

# T1 - Sneaky
nmap -T1 192.0.2.10

# T2 - Polite (slow, less bandwidth)
nmap -T2 192.0.2.10

# T3 - Normal (default)
nmap -T3 192.0.2.10

# T4 - Aggressive (fast, assumes good network)
nmap -T4 192.0.2.10

# T5 - Insane (fastest, may miss ports)
nmap -T5 192.0.2.10
```

**Recommended:**
- **T4** (-T4) - Fast scanning on local networks
- **T3** (-T3) - Default, balanced
- **T2** (-T2) - On slow/unreliable networks

---

## Skip Host Discovery

Sometimes firewalls block ping probes:

```bash
# Skip ping, assume host is up
nmap -Pn 192.0.2.10

# Useful when:
# - Target blocks ping (ICMP)
# - Firewall drops packets
# - You know host is alive
```

---

## Output Formats

### Save Scan Results

```bash
# -oN: Normal format (human-readable)
nmap -oN scan_results.txt 192.0.2.10

# -oX: XML format (for tools)
nmap -oX scan_results.xml 192.0.2.10

# -oG: Grepable format (for grep/awk)
nmap -oG scan_results.gnmap 192.0.2.10

# -oA: All formats at once (recommended!)
nmap -oA scan_results 192.0.2.10
# Creates: scan_results.nmap, scan_results.xml, scan_results.gnmap
```

### Verbose Output

```bash
# -v: Verbose mode
nmap -v 192.0.2.10

# -vv: Very verbose
nmap -vv 192.0.2.10

# -d: Debugging
nmap -d 192.0.2.10
```

---

## Practical Examples

### Quick Network Scan

```bash
# Fast scan of top 1000 ports
nmap -T4 -F 192.0.2.0/24

# Options:
# -T4: Aggressive timing
# -F: Fast mode (top 100 ports)
```

### Comprehensive Single Host Scan

```bash
# Full scan with service detection
sudo nmap -T4 -A -p- 192.0.2.10 -v

# Options:
# -T4: Fast timing
# -A: Aggressive scan (OS, version, scripts, traceroute)
# -p-: All ports
# -v: Verbose output
```

### Web Server Scan

```bash
# Scan web-specific ports
nmap -p 80,443,8080,8443 -sV 192.0.2.10

# With HTTP scripts
nmap -p 80,443 --script=http-* 192.0.2.10
```

### Find Specific Services

```bash
# Find SSH servers on network
nmap -p 22 --open 192.0.2.0/24

# Find web servers
nmap -p 80,443,8080 --open 192.0.2.0/24

# Find databases
nmap -p 3306,5432,1433 --open 192.0.2.0/24
```

---

## Script Scanning (NSE)

NMAP Scripting Engine (NSE) adds powerful functionality:

### Common Scripts

```bash
# Run default scripts
nmap -sC 192.0.2.10

# Run all scripts in category
nmap --script=vuln 192.0.2.10

# Run specific script
nmap --script=http-title 192.0.2.10

# Multiple scripts
nmap --script=ssh-brute,ssh-auth-methods 192.0.2.10
```

### Useful Script Categories

```bash
# Vulnerability detection
nmap --script=vuln 192.0.2.10

# Brute force attacks (labs only!)
nmap --script=brute 192.0.2.10

# Discovery
nmap --script=discovery 192.0.2.10

# Authentication bypass
nmap --script=auth 192.0.2.10
```

---

## Evasion Techniques

### Fragmentation

```bash
# Fragment packets
nmap -f 192.0.2.10

# Maximum fragment size
nmap --mtu 16 192.0.2.10
```

### Decoy Scans

```bash
# Use decoy IPs to mask your scan
nmap -D RND:10 192.0.2.10

# Specific decoys
nmap -D 192.0.2.50,192.0.2.51,ME,192.0.2.52 192.0.2.10
```

### Source Port Spoofing

```bash
# Use source port 53 (DNS)
nmap --source-port 53 192.0.2.10

# Some firewalls allow port 53
```

---

## Firewall/IDS Evasion

```bash
# Slow scan to evade IDS
nmap -T1 -f --randomize-hosts 192.0.2.0/24

# Options:
# -T1: Slow timing
# -f: Fragment packets
# --randomize-hosts: Random order
```

---

## Real-World Scenarios

### Scenario 1: Initial Network Discovery

```bash
# Step 1: Find alive hosts
nmap -sn 192.0.2.0/24 -oA discovery

# Step 2: Quick port scan of alive hosts
nmap -T4 -F -iL alive_hosts.txt -oA quick_scan

# Step 3: Detailed scan of interesting hosts
nmap -T4 -A -p- 192.0.2.10 -oA detailed_scan
```

### Scenario 2: Web Application Assessment

```bash
# Find web servers
nmap -p 80,443,8080,8443 --open 192.0.2.0/24

# Enumerate web technologies
nmap -p 80,443 -sV --script=http-enum,http-headers 192.0.2.10

# Check for vulnerabilities
nmap -p 80,443 --script=http-vuln-* 192.0.2.10
```

### Scenario 3: CTF/HackTheBox

```bash
# Full aggressive scan
sudo nmap -T4 -A -p- TARGET_IP -v -oA htb_scan

# Check for common vulnerabilities
nmap --script=vuln TARGET_IP

# Enumerate specific service
nmap -p 445 --script=smb-* TARGET_IP
```

---

## NMAP Output Analysis

### Reading NMAP Results

```
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 8.2p1 Ubuntu
80/tcp    open  http    nginx 1.18.0
443/tcp   open  https   nginx 1.18.0
3306/tcp  open  mysql   MySQL 8.0.23
```

**Key Information:**
- **PORT**: Port number and protocol
- **STATE**: open, closed, filtered
- **SERVICE**: Service name
- **VERSION**: Software and version

### Using with Other Tools

```bash
# Extract open ports for further testing
grep "open" scan_results.gnmap | awk '{print $2}' > open_ports.txt

# Feed to other tools
cat open_ports.txt | while read port; do
    echo "Testing port $port..."
done
```

---

## Advanced Tool: Smap

**Smap** is a faster NMAP alternative:

```bash
# Install Smap
git clone https://github.com/s0md3v/Smap
cd Smap
pip3 install -r requirements.txt

# Usage (similar to NMAP)
python3 smap.py 192.0.2.10
```

---

## Best Practices

### Do's ✅
- Get written authorization before scanning
- Use `-oA` to save all output formats
- Start with discovery scan, then detailed
- Use verbose mode (-v) to monitor progress
- Scan during approved maintenance windows

### Don'ts ❌
- Never scan networks without permission
- Don't use aggressive scans on production
- Avoid scanning the entire internet
- Don't ignore rate limiting
- Never assume it's legal to scan

---

## Common Errors & Solutions

### "Packet filtered"

```bash
# Target has firewall
# Solution: Try with -Pn flag
nmap -Pn 192.0.2.10
```

### "Too many open files"

```bash
# Increase file descriptor limit
ulimit -n 4096
```

### "Permission denied"

```bash
# Some scans require root
sudo nmap -sS 192.0.2.10
```

---

## Practice Exercises

1. Scan scanme.nmap.org (legally authorized!)
2. Find all web servers on your home network
3. Create a script to scan and report open ports
4. Try different timing templates and compare speeds
5. Use NSE scripts to enumerate SMB shares (in lab)

---

## Quick Reference

### Essential Commands

```bash
# Basic scan
nmap 192.0.2.10

# Fast comprehensive scan
sudo nmap -T4 -A -p- 192.0.2.10 -v

# Save all formats
nmap -oA output 192.0.2.10

# Stealth scan
sudo nmap -sS -T2 192.0.2.10

# Service versions
nmap -sV 192.0.2.10
```

---

## See Also

- [Network Services](network-services.md) - What services NMAP finds
- [Wireshark Basics](wireshark-basics.md) - Analyze NMAP traffic
- [Online Brute Forcing](online-brute-forcing.md) - Test discovered services
