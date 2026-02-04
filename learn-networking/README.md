# 🌐 Learn Networking - Security & Penetration Testing Focus

Welcome to networking fundamentals with a focus on security testing and penetration testing!

---

## 📚 Learning Path

> 💡 **This is a pentesting-oriented networking guide - perfect for aspiring security professionals!**

### 🟢 Beginner Level - Network Fundamentals

1. **[TCP/IP Fundamentals](tcpip-fundamentals.md)** - OSI model, IP addressing, subnetting, TCP vs UDP
2. **[Network Services](network-services.md)** - SSH, HTTP/HTTPS, FTP basics
3. **[Wireshark Basics](wireshark-basics.md)** - Network traffic analysis fundamentals

### 🟡 Intermediate Level - Reconnaissance & Scanning

4. **[NMAP Scanning](nmap-scanning.md)** - Port scanning, service detection, OS fingerprinting
5. **[Text Manipulation for Security](text-manipulation.md)** - Processing network data and logs

### 🔴 Advanced Level - Penetration Testing

6. **[Online Brute Forcing](online-brute-forcing.md)** - Hydra, MitM attacks (educational)
7. **[Offline Brute Forcing](offline-brute-forcing.md)** - Password cracking techniques
8. **[Payloads & Exploitation](payloads.md)** - Understanding attack payloads
9. **[Steganography](steganography.md)** - Hidden data detection and extraction

---

## 🎯 Who This Is For

- 🔍 **Aspiring Pentesters** - Learn network hacking basics
- 🛡️ **Security Enthusiasts** - Understand offensive security
- 📚 **Students** - Practical labs and CTF preparation
- 🔐 **Defenders** - Know what attackers do

---

## ⚠️ Legal & Ethical Notice

**CRITICAL:** All techniques in this guide are for:
- ✅ **Authorized testing** on your own systems
- ✅ **Educational purposes** in controlled labs
- ✅ **Ethical hacking** with explicit permission
- ✅ **Defensive security** understanding

❌ **NEVER** use these techniques on systems you don't own without written authorization!

**Unauthorized access to computer systems is illegal in most countries and can result in criminal charges.**

---

## 🚀 Prerequisites

### Required Knowledge
- Basic Linux command line (see [Learn Linux](../learn-linux/))
- Understanding of TCP/IP basics
- Basic terminal navigation

### Tools Needed
- Linux environment (Kali Linux recommended)
- **Nmap** - Network scanner
- **Wireshark** - Packet analyzer
- **Hydra** - Brute force tool (for labs)

### Lab Environment Setup

**Option 1: TryHackMe (Recommended for Beginners)**
```bash
# Visit https://tryhackme.com/
# Safe, legal environment to practice
```

**Option 2: HackTheBox**
```bash
# Visit https://www.hackthebox.com/
# More challenging, great for practice
```

**Option 3: Local Lab**
```bash
# Set up VirtualBox/VMware with:
# - Kali Linux (attacker)
# - Metasploitable 2 (target)
# ONLY practice on machines you own!
```

---

## 📖 Learning Progression

```
Week 1-2: Network Services & Wireshark
   ↓
Week 3-4: NMAP Scanning & Text Processing
   ↓
Week 5-6: Online Brute Forcing (in labs!)
   ↓
Week 7-8: Advanced Topics (Payloads, Steganography)
```

---

## 🛠️ Tool Installation

### On Kali Linux (Pre-installed)
```bash
# Most tools come pre-installed
nmap --version
wireshark --version
hydra -h
```

### On Ubuntu/Debian
```bash
# Install required tools
sudo apt update
sudo apt install nmap wireshark hydra binwalk steghide

# For Wireshark, add user to wireshark group
sudo usermod -aG wireshark $USER
```

### On macOS
```bash
# Using Homebrew
brew install nmap wireshark hydra
```

---

## 🎓 Certification Paths

This guide prepares you for:
- **CEH** (Certified Ethical Hacker)
- **OSCP** (Offensive Security Certified Professional)
- **eJPT** (eLearnSecurity Junior Penetration Tester)
- **PNPT** (Practical Network Penetration Tester)

---

## 📚 Additional Free Resources

### Online Learning
- **TryHackMe** - https://tryhackme.com/ (Best for beginners)
- **HackTheBox** - https://www.hackthebox.com/
- **OverTheWire** - https://overthewire.org/wargames/
- **PentesterLab** - https://pentesterlab.com/

### Documentation
- **Nmap NSE Scripts** - https://nmap.org/nsedoc/
- **Wireshark User Guide** - https://www.wireshark.org/docs/
- **OWASP Testing Guide** - https://owasp.org/www-project-web-security-testing-guide/

### YouTube Channels
- **NetworkChuck** - Networking basics
- **IppSec** - HackTheBox walkthroughs
- **John Hammond** - CTF and pentesting
- **The Cyber Mentor** - Ethical hacking tutorials

---

## 🔐 Security Testing Mindset

### Always Ask:
1. **Do I have permission?** (Written authorization required)
2. **Am I in a safe environment?** (Isolated lab preferred)
3. **What am I learning?** (Focus on defense, not offense)
4. **What's the legal risk?** (Know your local laws)

### Best Practices:
- ✅ Use isolated lab environments
- ✅ Document all testing activities
- ✅ Get written permission before testing
- ✅ Respect scope limitations
- ✅ Report findings responsibly

---

## 🎯 What You'll Learn

### Network Fundamentals
- How network services work (SSH, FTP, HTTP)
- Understanding ports and protocols
- Network traffic analysis basics

### Reconnaissance
- Port scanning techniques
- Service enumeration
- OS fingerprinting
- Banner grabbing

### Attack Techniques (For Defense!)
- Brute force attack methods
- Man-in-the-Middle attacks
- Payload delivery mechanisms
- Steganography detection

### Defense Applications
- Identifying attack signatures
- Recognizing suspicious network activity
- Understanding attacker methodology
- Hardening network services

---

## 💡 Study Tips

1. **Practice in Labs** - Never on production systems!
2. **Take Notes** - Document every command and result
3. **Understand, Don't Memorize** - Know WHY, not just HOW
4. **Learn to Defend** - Think like an attacker to defend better
5. **Stay Legal** - Always get authorization

---

## 🤝 Contributing to Defense

Use these skills to:
- Secure your own networks
- Help organizations improve security
- Report vulnerabilities responsibly
- Educate others about security
- Build better defenses

---

**Ready to start?** Begin with [Network Services →](network-services.md)

**Remember:** With great power comes great responsibility. Use these skills ethically!
