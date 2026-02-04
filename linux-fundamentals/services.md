# Services Management

Linux service management, network verification, and troubleshooting using `service` command and `netstat`.

## Service Management (Legacy Method)

### Starting Services

```bash
# Start Apache web server
service apache2 start
```

### Checking Service Status

```bash
# Check status of all services
service --status-all | grep apache
```

**Output Symbols:**
- `[+]` - Service is currently running (e.g., `[+] apache2`)
- `[-]` - Service is stopped (e.g., `[-] apache-htcacheclean`)

### Common Service Operations

```bash
# Start a service
service <service-name> start

# Stop a service
service <service-name> stop

# Restart a service
service <service-name> restart

# Check service status
service <service-name> status

# Reload configuration without restart
service <service-name> reload
```

## Modern Service Management (systemctl)

While the extracted notes focused on the legacy `service` command, modern Linux distributions use `systemctl`:

```bash
# Start service
sudo systemctl start apache2

# Stop service
sudo systemctl stop apache2

# Restart service
sudo systemctl restart apache2

# Check status
systemctl status apache2

# Enable service to start on boot
sudo systemctl enable apache2

# Disable service from starting on boot
sudo systemctl disable apache2

# Check if service is enabled
systemctl is-enabled apache2

# Check if service is active
systemctl is-active apache2

# List all running services
systemctl list-units --type=service --state=running

# List all services
systemctl list-units --type=service

# View service logs
journalctl -u apache2
```

## Network Verification with netstat

### Basic netstat Usage

```bash
# View all TCP connections with process info
netstat -tapn
```

**Flags explained:**
- `-t` - TCP ports
- `-a` - All ports (listening and non-listening)
- `-p` - Show PID and program name
- `-n` - Show numerical addresses (don't resolve hostnames)

### Check Specific Services

#### Apache Web Server

```bash
netstat -tapn | grep -i apache
```

**Expected output:**
```bash
tcp6    0    0 :::80    :::*    LISTEN    16560/apache2
```

This shows Apache is listening on port 80.

#### SSH Server

```bash
netstat -tapn | grep -i ssh
```

**Expected ports:**
- Port 22 (default SSH)

### Common netstat Patterns

```bash
# Check if specific port is listening
netstat -tapn | grep :80

# Show all listening ports
netstat -tuln

# Show all established connections
netstat -tpn | grep ESTABLISHED

# Count connections by state
netstat -ant | awk '{print $6}' | sort | uniq -c | sort -nr

# Find which process is using a port
sudo netstat -tapn | grep :8080
```

## Modern Alternative: ss Command

```bash
# Show all TCP connections
ss -tapn

# Show listening sockets
ss -tln

# Show process using port
ss -tapn | grep :80

# Statistics
ss -s

# Show all sockets
ss -a

# Filter by state
ss state established

# Show UDP sockets
ss -uan
```

## FTP Configuration

### FTP Connection Syntax

```bash
# Format
ftp://<username>:<password>@<host>

# Example
ftp://FC2e125c:AN9vm_TQ5ut+JB@ftp-pro.houston.softwaregrp.com
```

### FTP Command Line Client

```bash
# Connect to FTP server
ftp ftp.example.com

# Connect with username
ftp user@ftp.example.com

# Download file
ftp> get filename.txt

# Upload file
ftp> put localfile.txt

# List files
ftp> ls

# Change directory
ftp> cd /path/to/directory

# Quit
ftp> bye
```

## Service File Locations

### Finding Service Files

```bash
# Find web server index files
find /var -name index.html -type f
```

**Common locations:**
- `/var/www/html/index.html` - Apache default
- `/var/lib/inetsim/http/wwwroot/index.html` - InetSim
- `/usr/share/nginx/html/index.html` - Nginx default

### Service Configuration Locations

```bash
# Apache/Apache2
/etc/apache2/
/etc/httpd/

# Nginx
/etc/nginx/

# SSH
/etc/ssh/

# FTP (vsftpd)
/etc/vsftpd.conf

# MySQL/MariaDB
/etc/mysql/
/etc/my.cnf
```

## Troubleshooting

### Service Won't Start

```bash
# Check service status
systemctl status <service>

# View recent logs
journalctl -u <service> -n 50

# Check configuration syntax (Apache)
apachectl configtest

# Check configuration syntax (Nginx)
nginx -t

# Check if port is already in use
sudo netstat -tapn | grep :<port>
```

### Port Already in Use

```bash
# Find process using port 80
sudo netstat -tapn | grep :80

# or with  ss
sudo ss -tapn | grep :80

# Kill process
sudo kill <PID>

# Force kill if needed
sudo kill -9 <PID>
```

### Permission Issues

```bash
# Check service file permissions
ls -la /var/www/html/

# Fix ownership to web server user
sudo chown -R www-data:www-data /var/www/html/

# Fix permissions (files: 644, directories: 755)
find /var/www/html -type f -exec chmod 644 {} \;
find /var/www/html -type d -exec chmod 755 {} \;
```

## Firewall & Port Management

### Check Firewall Status

```bash
# UFW (Ubuntu)
sudo ufw status

# firewalld (CentOS/RHEL)
sudo firewall-cmd --list-all

# iptables
sudo iptables -L -n
```

### Open Ports

```bash
# UFW
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# firewalld
sudo firewall-cmd --permanent --add-port=80/tcp
sudo firewall-cmd --reload

# iptables
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

## Security Considerations

### Service Hardening

```bash
# Disable unnecessary services
sudo systemctl disable <service>
sudo systemctl stop <service>

# List all enabled services
systemctl list-unit-files --type=service --state=enabled

# Remove service from startup
sudo systemctl mask <service>
```

### Monitor for Suspicious Activity

```bash
# Check for unauthorized listening services
sudo netstat -tapn | grep LISTEN

# Check for unusual connections
sudo netstat -tapn | grep ESTABLISHED

# Monitor service logs for errors
tail -f /var/log/syslog
journalctl -f
```

## Quick Reference

### Common Service Names

| Service | Package Name | Default Port |
|---------|-------------|--------------|
| Apache | apache2/httpd | 80, 443 |
| Nginx | nginx | 80, 443 |
| SSH | ssh/openssh-server | 22 |
| MySQL | mysql-server | 3306 |
| PostgreSQL | postgresql | 5432 |
| FTP | vsftpd | 21 |
| DNS | bind9 | 53 |
| SMTP | postfix | 25 |

### Port Reference

| Port | Service | Protocol |
|------|---------|----------|
| 20, 21 | FTP | TCP |
| 22 | SSH | TCP |
| 23 | Telnet | TCP |
| 25 | SMTP | TCP |
| 53 | DNS | TCP/UDP |
| 80 | HTTP | TCP |
| 443 | HTTPS | TCP |
| 3306 | MySQL | TCP |
| 3389 | RDP | TCP |
| 5432 | PostgreSQL | TCP |
| 8080 | HTTP Alt | TCP |

## See Also

- [Finding Files](finding-files.md) - Locating service files
- [Bash Scripting](bash-scripting.md) - Automating service management
- [Basic Commands](basic-commands.md) - File operations
