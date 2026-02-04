# Network Services

Understanding common network services - the foundation for security testing and system administration.

---

## 📚 Table of Contents

- [What are Network Services?](#what-are-network-services)
- [SSH (Port 22)](#ssh-port-22)
- [HTTP/HTTPS (Ports 80/443)](#httphttps-ports-80443)
- [FTP (Port 21)](#ftp-port-21)
- [File Transfer Methods](#file-transfer-methods)
- [Security Considerations](#security-considerations)

---

## What are Network Services?

**Network services** are applications that run on servers and listen on specific ports, allowing clients to connect and interact with them.

**Common Ports:**
- **22** - SSH (Secure Shell)
- **80** - HTTP (Web traffic)
- **443** - HTTPS (Secure web traffic)
- **21** - FTP (File Transfer Protocol)
- **25** - SMTP (Email)
- **3306** - MySQL (Database)

---

## SSH (Port 22)

**SSH** (Secure Shell) provides secure remote access to systems.

### Basic SSH Connection

```bash
# Connect to remote server
ssh username@hostname_or_ip

# Connect with specific port
ssh -p 2222 username@hostname_or_ip

# Connect with specific key
ssh -i ~/.ssh/id_rsa username@hostname_or_ip
```

### SSH Configuration

**Server configuration file:** `/etc/ssh/sshd_config`

```bash
# View SSH configuration
cat /etc/ssh/sshd_config

# Common settings to review:
# Port 22                          # SSH port
# PermitRootLogin no              # Disable root login (secure!)
# PasswordAuthentication yes      # Allow password auth
# PubkeyAuthentication yes        # Allow key-based auth
```

###  SSH Keys

**Generate SSH Keys:**
```bash
# Generate new SSH key pair
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

# Keys created:
# ~/.ssh/id_rsa       (private key - NEVER share!)
# ~/.ssh/id_rsa.pub   (public key - safe to share)
```

**Copy Public Key to Server:**
```bash
# Method 1: Using ssh-copy-id
ssh-copy-id username@remote_host

# Method 2: Manual copy
cat ~/.ssh/id_rsa.pub | ssh username@remote_host "cat >> ~/.ssh/authorized_keys"
```

### Converting PuTTY Keys (.ppk)

If you have a `.ppk` key from Windows/PuTTY:

```bash
# On Linux, convert .ppk to OpenSSH format:
# 1. Install PuTTY tools
sudo apt install putty-tools

# 2. Convert the key
puttygen privkey.ppk -O private-openssh -o id_rsa

# 3. Set correct permissions
chmod 600 id_rsa

# 4. Use the converted key
ssh -i id_rsa username@host
```

**Alternative:** Use PuTTYgen GUI:
1. File → Load private key
2. Conversions → Export OpenSSH key

---

## HTTP/HTTPS (Ports 80/443)

**HTTP** serves web content. **HTTPS** is the secure version using SSL/TLS.

### Apache Web Server

**Apache** is a popular web server.

**Default web directory:** `/var/www/html/`

```bash
# Start Apache
sudo systemctl start apache2

# Stop Apache
sudo systemctl stop apache2

# Check status
sudo systemctl status apache2

# Enable on boot
sudo systemctl enable apache2
```

**Create a simple web page:**
```bash
# Navigate to web directory
cd /var/www/html/

# Create index page
echo "<h1>Hello, World!</h1>" | sudo tee index.html

# Test in browser
# Visit: http://your_server_ip/
```

### Temporary Web Server with Python

For quick file sharing or testing:

```bash
# Python 3 (recommended)
sudo python3 -m http.server 80

# Python 2 (legacy)
sudo python -m SimpleHTTPServer 80

# Without sudo (uses port 8000)
python3 -m http.server

# Access at: http://localhost:8000
```

**Security Note:** SimpleHTTPServer has NO authentication - don't use in production!

---

## FTP (Port 21)

**FTP** transfers files between systems.

### FTP Client Commands

```bash
# Connect to FTP server
ftp ftp.example.com

# Login (will prompt for username/password)
Username: demo_user
Password: demo_password_123

# Navigate directories
cd /path/to/directory
ls

# Download file
get filename.txt

# Upload file
put localfile.txt

# Download multiple files
mget *.txt

# Exit
quit
```

### Anonymous FTP

Some servers allow anonymous access:

```bash
ftp ftp.example.com
Username: anonymous
Password: (your email or just press enter)
```

### FTP with wget

Download entire FTP directory:

```bash
# Download all files from FTP
wget -m ftp://username:password@ftp.example.com/

# Example with dummy credentials
wget -m ftp://demo_user:demo_pass@ftp.example.com/public/
```

**⚠️ Security Warning:** Passwords in URLs are visible in command history! Use:
```bash
# Better: Create .netrc file for credentials
echo "machine ftp.example.com login demo_user password demo_pass" >> ~/.netrc
chmod 600 ~/.netrc

# Then use without credentials in URL
wget -m ftp://ftp.example.com/
```

---

## File Transfer Methods

### SCP (Secure Copy)

Transfer files securely over SSH.

**Copy FROM remote server:**
```bash
# Basic syntax Linux:
scp username@remote_host:/path/to/file /local/destination/

# Example - Copy log file to Linux
scp demo_user@192.0.2.10:/var/log/auth.log.2 /home/user/logs/

# Example - Copy to Windows (from Windows cmd)
scp demo_user@192.0.2.10:/var/log/auth.log.2 C:\Users\username\Downloads\
```

**Copy TO remote server:**
```bash
# Copy local file to remote server
scp /local/file.txt username@remote_host:/remote/path/

# Copy entire directory
scp -r /local/directory username@remote_host:/remote/path/
```

**SCP Options:**
```bash
# Use specific port
scp -P 2222 user@host:/file /local/

# Use specific key
scp -i ~/.ssh/id_rsa user@host:/file /local/

# Verbose output
scp -v user@host:/file /local/

# Compress during transfer
scp -C user@host:/file /local/
```

### SFTP (SSH File Transfer Protocol)

Interactive file transfer over SSH:

```bash
# Connect to SFTP
sftp username@remote_host

# SFTP commands
ls              # List remote files
lls             # List local files
cd /path        # Change remote directory
lcd /path       # Change local directory
get file.txt    # Download file
put file.txt    # Upload file
mkdir dirname   # Create remote directory
rm file.txt     # Delete remote file
quit            # Exit
```

### rsync

Advanced file synchronization:

```bash
# Sync directory to remote server
rsync -avz /local/dir/ username@remote:/remote/dir/

# Sync from remote to local
rsync -avz username@remote:/remote/dir/ /local/dir/

# Options:
# -a: archive mode (preserves permissions, timestamps)
# -v: verbose
# -z: compress during transfer
# --delete: delete files in dest not in source
```

---

## Security Considerations

### SSH Security Best Practices

```bash
# 1. Disable root login
sudo nano /etc/ssh/sshd_config
# Set: PermitRootLogin no

# 2. Use key-based authentication only
# Set: PasswordAuthentication no
# Set: PubkeyAuthentication yes

# 3. Change default port
# Set: Port 2222  (use non-standard port)

# 4. Limit users who can SSH
# Add: AllowUsers alice bob charlie

# 5. Restart SSH service
sudo systemctl restart sshd
```

### FTP Security Issues

❌ **FTP Problems:**
- Credentials sent in plaintext
- Data sent unencrypted
- Vulnerable to sniffing

✅ **Better Alternatives:**
- **SFTP** (SSH File Transfer) - Encrypted
- **FTPS** (FTP over SSL) - Encrypted FTP
- **SCP** (Secure Copy) - Simple and secure

### Web Server Security

```bash
# Disable directory listing
sudo nano /etc/apache2/apache2.conf
# Add: Options -Indexes

# Set proper file permissions
sudo chmod 755 /var/www/html
sudo chmod 644 /var/www/html/index.html

# Keep Apache updated
sudo apt update
sudo apt upgrade apache2
```

---

## Common Issues & Troubleshooting

### SSH Connection Refused

```bash
# Check if SSH service is running
sudo systemctl status ssh

# Check if port 22 is open
sudo netstat -tlnp | grep 22

# Check firewall
sudo ufw status
sudo ufw allow 22/tcp
```

### Permission Denied (SCP/SFTP)

```bash
# Check file permissions on remote server
ls -l /path/to/file

# Check SSH key permissions
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub
```

### Apache Won't Start

```bash
# Check for errors
sudo systemctl status apache2

# Check Apache configuration
sudo apache2ctl configtest

# Check if port 80 is already in use
sudo netstat -tlnp | grep :80
```

---

##Practice Exercises

1. Set up SSH key-based authentication
2. Configure Apache to serve a simple website
3. Transfer files using SCP between two systems
4. Set up a secure SFTP server
5. Harden SSH configuration on a test server

---

## Quick Reference

### Port Cheat Sheet

| Service | Port | Protocol | Encrypted? |
|---------|------|----------|------------|
| SSH | 22 | TCP | ✅ Yes |
| FTP | 21 | TCP | ❌ No |
| SFTP | 22 | TCP | ✅ Yes |
| HTTP | 80 | TCP | ❌ No |
| HTTPS | 443 | TCP | ✅ Yes |
| SMTP | 25 | TCP | ❌ No |
| MySQL | 3306 | TCP | ⚠️ Optional |

---

## See Also

- [NMAP Scanning](nmap-scanning.md) - Discover services
- [Wireshark Basics](wireshark-basics.md) - Analyze traffic
- [Online Brute Forcing](online-brute-forcing.md) - Test service security
