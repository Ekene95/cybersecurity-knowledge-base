# Finding Files & Permissions

Advanced file search techniques with focus on permissions and security checks.

## Finding Files by Permissions

### Search for Files with Specific Permissions

```bash
# Find character device files with permission 666 in /dev
find /dev -type c -perm 666
```

**Explanation:**
- `/dev` - Search directory (device files location)
- `-type c` - Only character device files
- `-perm 666` - Exact permission match (rw-rw-rw-)

### Permission-Based Security Checks

```bash
# Find SUID files (potential security risk)
find / -perm -4000 -type f 2>/dev/null

# Find SGID files
find / -perm -2000 -type f 2>/dev/null

# Find files with sticky bit
find / -perm -1000 -type d 2>/dev/null

# Find world-writable files (security concern)
find / -perm -002 -type f 2>/dev/null

# Find world-writable directories
find / -perm -002 -type d 2>/dev/null
```

## Advanced Permission Searches

### Octal Permission Matching

```bash
# Exact permission match - 755
find /path -perm 755

# At least these permissions (symbolic: -rwxr-xr-x)
find /path -perm -755

# Any of these permissions
find /path -perm /755
```

### Symbolic Permission Matching

```bash
# Find files readable by others
find /path -perm -o=r

# Find files writable by group
find /path -perm -g=w

# Find files executable by user
find /path -perm -u=x

# Find files with no permissions for others
find /path ! -perm /o=rwx
```

## Combining Searches

### Multiple Criteria

```bash
# Files owned by specific user with specific permissions
find /home -user alice -perm 644

# Large world-writable files
find / -perm -002 -size +10M -type f 2>/dev/null

# Recently modified SUID files (security audit)
find / -perm -4000 -mtime -7 -type f 2>/dev/null
```

### Logical Operators

```bash
# Files modified less than 7 days OR larger than 100MB
find /path \( -mtime -7 -o -size +100M \)

# Files NOT owned by root
find /path ! -user root

# Files with specific owner AND permissions
find /path -user www-data -perm 644
```

## File Ownership Searches

### By User

```bash
# Find files owned by specific user
find /path -user username

# Find files NOT owned by root
find /path ! -user root

# Find files with no valid owner (UID doesn't exist)
find /path -nouser
```

### By Group

```bash
# Find files owned by specific group
find /path -group groupname

# Find files with no valid group
find /path -nogroup
```

## Link-Related Operations

### Finding Links

```bash
# Find all symbolic links
find /path -type l

# Find broken symbolic links
find /path -type l ! -exec test -e {} \; -print

# Find hard links to a file
find /path -samefile /path/to/file

# Find files with more than 1 hard link
find /path -type f -links +1
```

### Creating Links

```bash
# Create symbolic link
ln -s /path/to/target linkname

# Create hard link
ln /path/to/target linkname

# Create symbolic link with absolute path
ln -s $(pwd)/target absolute_link

# Force overwrite existing link
ln -sf /path/to/new_target linkname
```

## Web Root & Service File Searches

### Locate Web Server Files

```bash
# Find index.html files (web servers)
find /var -name index.html -type f

# Common locations:
# /var/www/html/index.html (Apache)
# /var/lib/inetsim/http/wwwroot/index.html (InetSim)
# /usr/share/nginx/html/index.html (Nginx)
```

### Configuration File Searches

```bash
# Find Apache configuration
find /etc -name "apache*.conf" 2>/dev/null

# Find Nginx configuration
find /etc -name "nginx.conf" 2>/dev/null

# Find SSH configuration
find /etc -name "sshd_config" 2>/dev/null

# Find all .conf files
find /etc -name "*.conf" -type f 2>/dev/null
```

## Security Auditing

### Common Security Checks

```bash
# Find SUID binaries (potential privilege escalation)
find / -perm -4000 -type f -exec ls -la {} \; 2>/dev/null

# Find world-writable files in /etc
find /etc -perm -002 -type f 2>/dev/null

# Find files modified in last 24 hours (incident response)
find / -mtime -1 -type f 2>/dev/null

# Find files with no owner (orphaned files)
find / -nouser -o -nogroup 2>/dev/null

# Find hidden files in home directories
find /home -name ".*" -type f 2>/dev/null
```

### Pentesting File Searches

```bash
# Find SSH keys
find / -name "id_rsa*" -o -name "id_dsa*" -o -name "id_ecdsa*" 2>/dev/null

# Find password files
find / -name "passwd" -o -name "shadow" 2>/dev/null

# Find database files
find / -name "*.db" -o -name "*.sqlite*" 2>/dev/null

# Find configuration with credentials
find /var/www -name "config*.php" -o -name "wp-config.php" 2>/dev/null

# Find backup files
find / -name "*.bak" -o -name "*.backup" -o -name "*~" 2>/dev/null
```

## Performance Optimization

### Limiting Search Scope

```bash
# Limit search depth
find /etc -maxdepth 2 -name "*.conf"

# Stay within same filesystem
find /var -xdev -name "*.log"

# Exclude directories
find / -path /proc -prune -o -path /sys -prune -o -name "file" -print
```

### Faster Searches with locate

```bash
# Update locate database first
sudo updatedb

# Quick search (much faster than find)
locate filename

# Regex search with locate
locate -r '\.conf$'
```

## Practical Examples

### Find and Copy

```bash
# Find and copy all .conf files to backup directory
find /etc -name "*.conf" -exec cp {} /backup/configs/ \;
```

### Find and Archive

```bash
# Find and tar all logs older than 30 days
find /var/log -name "*.log" -mtime +30 -print | tar czf old_logs.tar.gz -T -
```

### Find and Change Permissions

```bash
# Fix world-writable files
find /var/www -perm -002 -type f -exec chmod 644 {} \;

# Fix executable files
find /var/www -name "*.sh" -type f -exec chmod 755 {} \;
```

## Tips & Best Practices

1. **Always redirect errors** with `2>/dev/null` when searching from `/`
2. **Use `-maxdepth`** to limit search scope and improve performance
3. **Combine with `-exec` carefully** - test with `-print` first
4. **Use `locate` for filename searches** when real-time isn't critical
5. **Check SUID/SGID files regularly** for security auditing
6. **Document permission changes** when fixing security issues

## See Also

- [Basic Commands](basic-commands.md) - Introduction to find command
- [Services](services.md) - Finding service-related files
- [Bash Scripting](bash-scripting.md) - Automating file searches
