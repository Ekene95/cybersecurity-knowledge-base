# Archiving & Compression

Complete guide to creating, managing, and compressing archives using `tar` and related utilities.

## Appending Files to Existing Archives

Move files into an existing tar archive:

```bash
sudo tar rvf /tmp/home.tar /etc/yum.repos.d
```

**Important Notes:**
- The `-r` option appends files to the end of an existing tarfile
- **Does NOT work** with compressed tarballs (`.tar.gz` or `.tar.bz2`)
- Only works with uncompressed `.tar` files

## Viewing Archive Contents

Check what's inside a tar file:

```bash
# View contents with details
tar tvf /tmp/home.tar

# Verify specific files
tar tvf /tmp/files.tar
```

**Output includes:**
- File permissions
- Owner/group
- File sizes
- Modification timestamps
- Full file paths

## Creating and Compressing Archives

### Using bzip2 Compression

```bash
# Create and compress with bzip2
sudo tar cvjf /tmp/home.tar.bz2 /home
```

**Flags explained:**
- `c` - Create archive
- `v` - Verbose output
- `j` - Use bzip2 compression
- `f` - Specify filename

### Archive with Extended Attributes & SELinux

For system backups that preserve metadata:

```bash
sudo tar cvj --selinux --xattrs -f /tmp/extattr.tar.bz2 /home
```

**Additional Options:**
- `--selinux` - Include SELinux security contexts
- `--xattrs` - Include extended attributes
- `-f` - Specify the output filename

**Requirement:** Many operations require `sudo` when accessing system directories or metadata.

## Common tar Options

### Creating Archives

| Command | Description |
|---------|-------------|
| `tar cvf archive.tar files/` | Create uncompressed archive |
| `tar cvzf archive.tar.gz files/` | Create gzip compressed archive |
| `tar cvjf archive.tar.bz2 files/` | Create bzip2 compressed archive |
| `tar cvJf archive.tar.xz files/` | Create xz compressed archive |

### Extracting Archives

| Command | Description |
|---------|-------------|
| `tar xvf archive.tar` | Extract tar archive |
| `tar xvzf archive.tar.gz` | Extract gzip archive |
| `tar xvjf archive.tar.bz2` | Extract bzip2 archive |
| `tar xvJf archive.tar.xz` | Extract xz archive |
| `tar xvf archive.tar -C /path/` | Extract to specific directory |

### Listing Contents

| Command | Description |
|---------|-------------|
| `tar tvf archive.tar` | List contents (verbose) |
| `tar tf archive.tar` | List contents (simple) |
| `tar tvf archive.tar \| grep pattern` | Search for files in archive |

## Compression Comparison

| Method | Extension | Flag | Compression Ratio | Speed |
|--------|-----------|------|-------------------|-------|
| None | `.tar` | (none) | 1:1 | Fastest |
| gzip | `.tar.gz` | `z` | Good | Fast |
| bzip2 | `.tar.bz2` | `j` | Better | Medium |
| xz | `.tar.xz` | `J` | Best | Slower |

## Practical Examples

### Backup User Home Directories

```bash
# Basic backup
sudo tar cvzf /backup/home-$(date +%Y%m%d).tar.gz /home

# With SELinux and extended attributes
sudo tar cvj --selinux --xattrs -f /backup/home-full-$(date +%Y%m%d).tar.bz2 /home
```

### Backup System Configuration

```bash
# Backup /etc directory
sudo tar cvzf /backup/etc-$(date +%Y%m%d).tar.gz /etc

# Exclude certain directories
sudo tar cvzf /backup/etc-$(date +%Y%m%d).tar.gz --exclude=/etc/ssl/private /etc
```

### Extract Specific Files

```bash
# Extract a single file
tar xvf archive.tar path/to/specific/file

# Extract files matching pattern
tar xvf archive.tar --wildcards '*.conf'
```

### Verify Archive Integrity

```bash
# Test archive integrity
tar tvf archive.tar > /dev/null && echo "Archive OK" || echo "Archive corrupted"

# For compressed archives
tar tvzf archive.tar.gz > /dev/null && echo "Archive OK" || echo "Archive corrupted"
```

## Advanced Usage

### Split Large Archives

```bash
# Create and split archive into 100MB chunks
tar cvzf - /large/directory | split -b 100M - archive.tar.gz.part

# Reconstruct and extract
cat archive.tar.gz.part* | tar xvzf -
```

### Archive with Progress Bar

```bash
# Using pv (pipe viewer)
tar cvf - /directory | pv -s $(du -sb /directory | awk '{print $1}') | gzip > archive.tar.gz
```

### Incremental Backups

```bash
# Create snapshot file for incremental backup
tar cvzf backup-full.tar.gz --listed-incremental=snapshot.file /data

# Subsequent incremental backup
tar cvzf backup-inc1.tar.gz --listed-incremental=snapshot.file /data
```

## Tips & Best Practices

1. **Always use `-v` (verbose)** during creation to monitor progress
2. **Use absolute paths carefully** - they'll extract to absolute locations
3. **Test extracts** with `-t` before actually extracting (`-x`)
4. **Preserve permissions** - tar preserves permissions by default
5. **For backups**, include `--selinux` and `--xattrs` on SELinux systems
6. **Compression trade-offs**: gzip for speed, bzip2 for balance, xz for size
7. **Append operations** only work on uncompressed archives

## Common Errors & Solutions

| Error | Solution |
|-------|----------|
| "Cannot open: Permission denied" | Use `sudo` for system directories |
| "Cannot append to compressed archives" | Extract, modify, recompress |  
| "File changed while reading" | Files modified during archiving, usually safe to ignore |

## See Also

- [Basic Commands](basic-commands.md) - Find command basics
- [File Manipulation](file-manipulation.md) - Working with files
- [Encryption](encryption.md) - Encrypting archives
