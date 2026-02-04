# Useful Commands

Quick reference for handy Linux one-liners and commonly used commands.

## Random Data & Encoding

### Generate Random Base64 Strings

```bash
# Generate 120 lines of random base64 (16 char width)
head -n 120 /dev/urandom | base64 -w 16 > file.txt
```

**Use cases:**
- Generate test data
- Create dummy files
- Password generation base

### Decode Base64 from File

```bash
# Decode each line from file
for i in $(cat file.txt); do 
    echo $i | base64 -d
done
```

**Note:** If source is random data, decoded output will be gibberish (binary data).

## Quick Base64 Operations

```bash
# Encode string
echo "Hello World" | base64
# Output: SGVsbG8gV29ybGQK

# Decode string
echo "SGVsbG8gV29ybGQK" | base64 -d
# Output: Hello World

# Encode file
base64 file.txt > file.b64

# Decode file
base64 -d file.b64 > file.txt

# Encode without line wrapping
base64 -w 0 file.txt
```

## One-Liners Collection

### File Operations

```bash
# Find and delete old log files
find /var/log -name "*.log" -mtime +30 -delete

# Count files in directory
ls -1 | wc -l

# Find top 10 largest files
find / -type f -exec du -h {} + 2>/dev/null | sort -rh | head -10

# Find duplicate files by size
find . -type f -exec du -b {} + | sort -n | uniq -d -w10

# Batch rename files (add .bak extension)
for file in *.txt; do mv "$file" "$file.bak"; done
```

### System Information

```bash
# Show top 10 memory consumers
ps aux --sort=-%mem | head -11

# Show top 10 CPU consumers
ps aux --sort=-%cpu | head -11

# Show disk usage in human readable format
df -h | grep -v tmpfs

# Show directory sizes sorted
du -sh */ | sort -h

# Get public IP
curl ifconfig.me
```

### Network Operations

```bash
# Show open ports
sudo netstat -tulpn

# Test if port is open
nc -zv hostname port

# Download file with wget
wget -O filename URL

# Download with curl
curl -o filename URL

# Monitor network traffic
sudo iftop

# Show routing table
ip route show
```

### Text Processing

```bash
#Remove duplicate lines
sort file.txt | uniq

# Remove all blank lines
grep -v '^$' file.txt

# Show only duplicate lines
sort file.txt | uniq -d

# Number lines in file
nl file.txt

# Convert dos to unix line endings
dos2unix file.txt

# Convert tabs to spaces
expand file.txt
```

### Process Management

```bash
# Kill all processes by name
pkill process_name

# Monitor process in real-time
watch -n 1 ps aux | grep process_name

# Run command in background
nohup command &

# List all processes in tree format
pstree

# Show process details
ps -fp PID
```

### Archive & Compression

```bash
# Quick tar + gzip
tar czf archive.tar.gz directory/

# Quick extract
tar xzf archive.tar.gz

# Zip directory
zip -r archive.zip directory/

# Unzip file
unzip archive.zip

# Create split archive (100MB chunks)
tar czf - directory/ | split -b 100M - archive.tar.gz.
```

## Productivity Shortcuts

### History Tricks

```bash
# Re-run last command
!!

# Re-run last command with sudo
sudo !!

# Re-run command from history (number)
!123

# Re-run last command starting with 'git'
!git

# Search history
history | grep command

# Clear history
history -c
```

### Command Substitution

```bash
# Use output of command as argument
ls -l $(which python3)

# Alternative syntax
ls -l `which python3`

# Store command output in variable
FILES=$(ls *.txt)
```

### Aliases (Add to ~/.bashrc)

```bash
# Quick navigation
alias ..='cd ..'
alias ...='cd ../..'

# Safety nets
alias rm='rm -i'
alias mv='mv -i'
alias cp='cp -i'

# Shortcuts
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'

# Git shortcuts
alias gs='git status'
alias ga='git add'
alias gc='git commit'
alias gp='git push'

# System info
alias ports='netstat -tulanp'
alias meminfo='free -m -l -t'
alias psmem='ps auxf | sort -nr -k 4'
alias pscpu='ps auxf | sort -nr -k 3'
```

## Data Analysis Quick Commands

### Log Analysis

```bash
# Count unique IPs in access log
awk '{print $1}' access.log | sort | uniq -c | sort -nr

# Show 404 errors
grep " 404 " access.log | awk '{print $7}' | sort | uniq -c | sort -nr

# Count HTTP status codes
awk '{print $9}' access.log | sort | uniq -c | sort -nr

# Show top user agents
awk -F'"' '{print $6}' access.log | sort | uniq -c | sort -nr | head -20
```

### CSV/TSV Processing

```bash
# Extract specific column (comma-separated)
cut -d',' -f2 file.csv

# Show first 10 rows
head -10 file.csv

# Count rows
wc -l file.csv

# Remove header
tail -n +2 file.csv

# Convert CSV to TSV
tr ',' '\t' < file.csv > file.tsv
```

## Security Quick Commands

### Password Generation

```bash
# Random 20 char password
openssl rand -base64 20

# Alphanumeric password
tr -dc 'A-Za-z0-9' < /dev/urandom | head -c 20

# With special characters
tr -dc 'A-Za-z0-9!@#$%^&*' < /dev/urandom | head -c 20
```

### Hash Generation

```bash
# MD5 hash
echo -n "text" | md5sum

# SHA-256 hash
echo -n "text" | sha256sum

# File hash
sha256sum file.txt
```

### File Permissions

```bash
# Recursive chmod
chmod -R 755 directory/

# Change owner recursively
chown -R user:group directory/

# Find and fix permissions
find . -type f -exec chmod 644 {} \;
find . -type d -exec chmod 755 {} \;
```

## Disk Space Management

### Find Space Hogs

```bash
# Top 10 largest directories
du -h / 2>/dev/null | sort -rh | head -10

# Disk usage by directory
ncdu

# Find files larger than 100MB
find / -type f -size +100M -exec ls -lh {} \; 2>/dev/null

# Show inode usage
df -i
```

### Clean Up

```bash
# Remove old log files
find /var/log -type f -name "*.log" -mtime +30 -delete

# Clean package cache (Debian/Ubuntu)
sudo apt-get clean

# Clean package cache (RedHat/CentOS)
sudo yum clean all

# Remove orphaned packages (Debian/Ubuntu)
sudo apt autoremove
```

## Performance Monitoring

```bash
# Real-time system stats
htop

# IO statistics
iostat -x 1

# Virtual memory statistics
vmstat 1

# Disk IO activity
iotop

# Network bandwidth
iftop

# Process monitoring
top
```

## Useful sed/awk One-Liners

### SED Operations

```bash
# Replace in place
sed -i 's/old/new/g' file.txt

# Delete lines containing pattern
sed '/pattern/d' file.txt

# Show lines between patterns
sed -n '/start/,/end/p' file.txt

# Add line after pattern
sed '/pattern/a\new line' file.txt
```

### AWK Operations

```bash
# Print specific columns
awk '{print $1, $3}' file.txt

# Sum column
awk '{sum+=$1} END {print sum}' file.txt

# Filter by condition
awk '$3 > 100' file.txt

# Split by custom delimiter
awk -F: '{print $1}' /etc/passwd
```

## See Also

- [File Manipulation](file-manipulation.md) - Advanced text processing
- [Basic Commands](basic-commands.md) - File operations
- [Bash Scripting](bash-scripting.md) - Automation
- [Caesar Cipher](caesar-cipher.md) - Character manipulation
