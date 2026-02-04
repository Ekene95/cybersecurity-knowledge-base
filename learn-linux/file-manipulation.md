# File Manipulation & Log Analysis

Advanced text processing, filtering, and log analysis techniques using Linux command-line tools.

## Log Analysis & Filtering

### Searching for Specific Patterns

Search logs for IP addresses and file extensions:
```bash
# Search for IP and .sh files
cat audit.log | grep 192.0.2.100 | grep "\.sh"

# Search for IP and .tar.gz files
cat audit.log | grep 192.0.2.100 | grep -e "\.tar\.gz"
```

### Extracting and Counting with AWK

Extract failed login attempts and count by IP:
```bash
# Extract IPs from failed logins (3rd column from end)
cat auth.log | grep Failed | awk '{print $(NF-3)}' | sort | uniq -c | grep 49

# Count unique IPs with 49 failed attempts
cat auth.log.2 | grep Failed | grep 192.0.2.200 | awk '{print $(NF-5)}' | sort | uniq -c | sort -n | wc -l

# Alternative: NF-6 or NF-3 depending on line format
cat auth.log | grep Failed | awk '{print $(NF-6)}' | sort | uniq -c | sort -n
```

## AWK - Advanced Pattern Matching

### Case-Insensitive Search with Multiple Conditions

Find lines containing two different words:
```bash
# Find lines with both "Failed" and "root"
awk 'BEGIN{IGNORECASE=1} /Failed/&&/root/{print $0}' auth.log | wc -l

# Complex example: Failed + invalid, then extract IPs
cat auth.log | awk 'BEGIN{IGNORECASE=1} /Failed/&&/invalid/{print $0}' | awk '{print $(NF-3)}' | sort | uniq -c | wc -l
```

## SED - Stream Editor

### Pattern Matching and Filtering

Find lines containing specific words:
```bash
# Generic pattern: find lines with <word>
cat <filename> | sed -n '/<word>/p' | wc -l

# Example: Extract invalid user attempts, then search for IP
cat auth.log | grep -i "failed password for invalid user" > invaliduser.txt
cat invaliduser.txt | sed -n '/192.0.2.50/p' | wc -l
```

## Advanced Grep Techniques

### Excluding Patterns

```bash
# Exclude lines containing "failed" (case-insensitive, whole word)
grep -wv -i "failed" auth.log | wc -l
```

### IP Address Extraction

#### Basic IP Regex
```bash
grep -o '[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}' sample.log
```

#### Strict IP Validation (Extended Regex)
```bash
# Validates IP ranges (0-255 for each octet)
grep -Eo '(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)' sample.log

# Combined with uniq and sort
grep -Eo '(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)' sample.log | uniq -c | sort
```

### Recursive Search

```bash
# Search across multiple files
grep -Er THM webserver.log SSHD.log

# Find "password" recursively, suppress errors
grep -iRn 'password' . 2>/dev/null
```

## Text Transformation with TR

### Case Conversion

```bash
# Convert to uppercase
echo lInuX | tr [:lower:] [:upper:]
# Output: LINUX
```

### Caesar Cipher Translation

```bash
# Shift cipher transformation
echo EADGT UGEWTKVA KU CNUQ PKEG | tr '[C-ZA-B]' '[A-Z]'
```

## Cut & AWK Combination

Extract and filter specific fields:
```bash
cat leekpeek_extract.txt | awk '{print $1}' | cut -d: -f1 | grep -i johnbryce | sort | uniq -ic | sort -n | wc -l
```

## Shell Wildcards & Pattern Matching

### Single Character Match (`?`)

```bash
# List 4-character directories in /etc
ls -d /etc/????
```

### Set/Range Match (`[]`)

```bash
# Files starting with 'l' or 'p'
ls /etc/dnf/modules.d/[lp]*

# Files starting with 'a', 'b', or 'c'
ls /usr/share/awk/[a-c]*
```

## Practical Examples

### Failed Login Analysis

```bash
# Count failed logins by IP, show top offenders
cat auth.log | grep "Failed password" | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr | head -20

# Find IPs with specific number of failed attempts
cat auth.log | grep Failed | awk '{print $(NF-3)}' | sort | uniq -c | awk '$1 == 49 {print $2}'
```

### Multi-Pattern Search

```bash
# Find lines matching multiple patterns (AND logic)
awk '/pattern1/ && /pattern2/ {print}' file.log

# Case-insensitive multi-pattern
awk 'BEGIN{IGNORECASE=1} /failed/ && /invalid/ {print}' auth.log
```

## Tips & Best Practices

1. **Use `-n` flag with grep** to show line numbers for easier debugging
2. **Pipe through `wc -l`** for quick counts
3. **Use `sort | uniq -c | sort -n`** pattern for frequency analysis
4. **Extended regex (`-E`)** provides more powerful pattern matching
5. **`$(NF-X)` in awk** counts from the end of line, useful for inconsistent log formats
6. **Redirect errors** with `2>/dev/null` to clean up output

## See Also

- [Grep & Egrep](grep-and-egrep.md) - Extended grep usage
- [Caesar Cipher & TR](caesar-cipher.md) - More text transformation
- [Bash Scripting](bash-scripting.md) - Automation examples
