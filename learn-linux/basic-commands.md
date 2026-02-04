# Basic Commands

Essential Linux terminal operations and file search commands.

## File Listing with Filters

### List files by modification time

```bash
# List .log files sorted by modification time (reverse, oldest first), directories only
ll -rtd *.log
```

**Flags explained:**
- `-r` - Reverse order
- `-t` - Sort by modification time  
- `-d` - List directories themselves, not their contents
- `*.log` - Only match .log files

## Finding Files

### Basic Find Syntax

```bash
# Find a file by name starting from root
find / -name aggregate.log -ls
```

### Common Permission Errors

When running `find` from root without sudo, you'll encounter permission denied errors:

```bash
find / -name aggregate.log -ls

# Common errors you'll see:
# find: '/var/cache/ldconfig': Permission denied
# find: '/var/log/krb5': Permission denied
# find: '/var/log/zypp': Permission denied
# find: '/etc/skel/.cache': Permission denied
```

**Solution:** Use `sudo` or redirect errors to `/dev/null`:

```bash
# With sudo
sudo find / -name aggregate.log -ls

# Suppress errors
find / -name aggregate.log -ls 2>/dev/null
```

## Find Command Reference

### Find by Name

```bash
# Find exact filename
find /path -name "filename.txt"

# Case-insensitive
find /path -iname "filename.txt"

# Find with wildcards
find /path -name "*.log"
```

### Find by Type

```bash
# Find only files
find /path -type f -name "*.txt"

# Find only directories
find /path -type d -name "backup"

# Find symbolic links
find /path -type l
```

### Find by Size

```bash
# Find files larger than 100MB
find /path -type f -size +100M

# Find files smaller than 1KB
find /path -type f -size -1k

# Find exact size (512-byte blocks)
find /path -type f -size 100c
```

### Find by Time

```bash
# Modified in last 7 days
find /path -mtime -7

# Modified more than 30 days ago
find /path -mtime +30

# Accessed in last 24 hours
find /path -atime -1

# Changed in last hour
find /path -cmin -60
```

### Find by Permissions

```bash
# Find files with permission 644
find /path -perm 644

# Find files with at least read permission for others
find /path -perm -004

# Find SUID files (security check)
find / -perm -4000 -type f 2>/dev/null

# Find world-writable files
find / -perm -002 -type f 2>/dev/null
```

### Find and Execute Commands

```bash
# Delete found files
find /path -name "*.tmp" -delete

# Execute command on found files
find /path -name "*.log" -exec ls -lh {} \;

# Execute with confirmation
find /path -name "*.bak" -ok rm {} \;

# Execute on multiple files at once (faster)
find /path -name "*.txt" -exec grep "pattern" {} +
```

## Locate Command

**Note:** The `locate` command wasn't covered in the extracted PDFs, but here's the reference:

```bash
# Update locate database
sudo updatedb

# Find files quickly
locate filename

# Case-insensitive search
locate -i filename

# Count matches
locate -c filename

# Show only existing files
locate -e filename
```

## Quick Reference

### Common Find Patterns

```bash
# Find and list all .conf files
find /etc -name "*.conf" -type f 2>/dev/null

# Find empty files
find /path -type f -empty

# Find empty directories
find /path -type d -empty

# Find files owned by specific user
find /path -user username

# Find files owned by specific group
find /path -group groupname

# Find files modified in last 24 hours
find /path -type f -mtime -1

# Find large files (>100MB) and show size
find / -type f -size +100M -exec ls -lh {} \; 2>/dev/null
```

### Performance Tips

```bash
# Limit search depth
find /path -maxdepth 2 -name "*.txt"

# Search only in specific filesystem
find /path -xdev -name "file"

# Exclude specific directories
find /path -name "*.log" -not -path "*/node_modules/*"
```

## File Operations

### Copy Files

```bash
# Copy file
cp source dest

# Copy directory recursively
cp -r source_dir dest_dir

# Copy preserving attributes
cp -a source dest

# Interactive copy (prompt before overwrite)
cp -i source dest
```

### Move/Rename Files

```bash
# Move file
mv source dest

# Rename file
mv oldname newname

# Move multiple files
mv file1 file2 file3 /dest/directory/
```

### Delete Files

```bash
# Delete file
rm filename

# Delete directory and contents
rm -r directory

# Force delete without confirmation
rm -f filename

# Interactive delete
rm -i filename

# Delete empty directory
rmdir directory
```

## See Also

- [Finding Files](finding-files.md) - Advanced file search with permissions
- [File Manipulation](file-manipulation.md) - Text processing and analysis
- [Archiving](archiving.md) - File compression and archiving
