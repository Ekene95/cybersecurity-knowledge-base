# Grep &Eger Egrep - Pattern Matching

Regular expression pattern matching using grep and egrep for finding URLs, IPs, and complex patterns.

## Egrep for URL Validation

### HTTP/HTTPS URL Pattern

```bash
# Match HTTP/HTTPS URLs with optional www
egrep '^http(s)?.{3}(www)?.+\..+$' filename.txt
```

**Pattern breakdown:**
- `^` - Start of line
- `http(s)?` - Match "http" with optional "s"
- `.{3}` - Exactly 3 characters (matches "://" plus one more)
- `(www)?` - Optional "www"
- `.+` - One or more characters
- `\.` - Literal dot
- `.+` - One or more characters (domain extension)
- `$` - End of line

**What it matches:**
- `http://example.com`
- `https://www.example.com`
- `https://subdomain.example.org`

## Grep vs Egrep vs Fgrep

### Grep (Basic Regular Expressions)

```bash
# Basic pattern matching
grep "pattern" file.txt

# Case-insensitive
grep -i "pattern" file.txt

# Show line numbers
grep -n "pattern" file.txt

# Invert match (show non-matching lines)
grep -v "pattern" file.txt

# Count matches
grep -c "pattern" file.txt

# Show only filename
grep -l "pattern" *.txt

# Recursive search
grep -r "pattern" /path/
```

### Egrep (Extended Regular Expressions)

```bash
# OR operator
egrep "pattern1|pattern2" file.txt

# Character classes
egrep "[0-9]{3}-[0-9]{4}" file.txt  # Phone numbers

# Groups
egrep "(error|warning|critical)" syslog

# Quantifiers
egrep "colou?r" file.txt  # Matches color or colour
```

### Fgrep (Fixed String / Fast Grep)

```bash
# Fast literal string matching (no regex)
fgrep "exact.string.with.dots" file.txt

# Good for searching special characters
fgrep "$(ls)" file.txt
```

## Extended Regex Patterns

### IP Address Matching

See [File Manipulation](file-manipulation.md) for comprehensive IP regex patterns:

```bash
# Basic IP pattern
grep -E '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' file.log

# Strict IP validation (0-255 range)
grep -Eo '(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)' file.log
```

### Email Address Pattern

```bash
# Basic email pattern
egrep '[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}' file.txt

# More strict pattern
egrep '^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$' emails.txt
```

### Phone Number Patterns

```bash
# US phone format: 123-456-7890
egrep '[0-9]{3}-[0-9]{3}-[0-9]{4}' file.txt

# Alternative format: (123) 456-7890
egrep '\([0-9]{3}\) [0-9]{3}-[0-9]{4}' file.txt

# Flexible pattern
egrep '(\+?1[-.\s]?)?\(?[0-9]{3}\)?[-.\s]?[0-9]{3}[-.\s]?[0-9]{4}' file.txt
```

## Advanced Grep Techniques

### Context Lines

```bash
# Show 2 lines before match
grep -B 2 "error" log.txt

# Show 3 lines after match
grep -A 3 "error" log.txt

# Show 2 lines before and after
grep -C 2 "error" log.txt
```

### Multiple Pattern Files

```bash
# Read patterns from file
grep -f patterns.txt input.txt

# Example patterns.txt:
# error
# warning
# critical
```

### Exclude Patterns

```bash
# Exclude specific pattern
grep "error" log.txt | grep -v "ignored"

# Exclude multiple patterns
grep "error" log.txt | grep -v "test" | grep -v "debug"
```

### Binary Files

```bash
# Search binary files
grep -a "text" binary_file

# Exclude binary files from search
grep -I "pattern" *

# Show binary files that match
grep -l "pattern" *
```

## Color Output

```bash
# Highlight matches
grep --color=auto "pattern" file.txt

# Always use color
grep --color=always "pattern" file.txt

# Set in bashrc for permanent
alias grep='grep --color=auto'
```

## Performance Optimization

### Fast Searches

```bash
# Use fgrep for literal strings
fgrep "exact string" large_file.txt

# Limit to first N matches
grep -m 10 "pattern" file.txt

# Stop reading after first match
grep -q "pattern" file.txt && echo "Found"
```

### Parallel Grep

```bash
# Search multiple files in parallel
find . -type f -print0 | xargs -0 -P 4 grep -l "pattern"
```

## Practical Examples

### Log Analysis

```bash
# Find errors in last hour
grep "$(date -d '1 hour ago' '+%Y-%m-%d %H')" /var/log/syslog | grep -i error

# Count error types
grep -i error /var/log/syslog | awk '{print $5}' | sort | uniq -c | sort -nr

# Find failed logins
grep "Failed password" /var/log/auth.log | egrep -o '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}'
```

### Security Scanning

```bash
# Find potential SQL injection
egrep -i "(union|select|insert|update|delete|drop|create).+(from|table|database)" access.log

# Find XSS attempts
egrep -i "(<script|javascript:|onerror=|onload=)" access.log

# Find path traversal attempts  
egrep "\.\./\.\." access.log
```

### Code Search

```bash
# Find TODO comments
grep -rn "TODO:" /path/to/code

# Find function definitions (Python)
egrep "^def [a-zA-Z_][a-zA-Z0-9_]*\(" *.py

# Find imports
egrep "^(import|from).+import" *.py
```

## Special Characters

### Escaping

```bash
# Escape special characters with backslash
grep "\." file.txt          # Literal dot
grep "\[" file.txt          # Literal bracket
grep "\$" file.txt          # Literal dollar sign
```

### Character Classes

```bash
# Digits
grep "[0-9]" file.txt
grep "[[:digit:]]" file.txt

# Letters
grep "[a-zA-Z]" file.txt
grep "[[:alpha:]]" file.txt

# Alphanumeric
grep "[[:alnum:]]" file.txt

# Whitespace
grep "[[:space:]]" file.txt

# Word characters
grep "\w" file.txt
```

## Quick Reference

### Common Flags

| Flag | Description |
|------|-------------|
| `-i` | Case-insensitive |
| `-v` | Invert match |
| `-c` | Count matches |
| `-n` | Show line numbers |
| `-l` | Show only filenames |
| `-r` | Recursive |
| `-E` | Extended regex (egrep) |
| `-F` | Fixed string (fgrep) |
| `-o` | Show only matching part |
| `-A N` | Show N lines after |
| `-B N` | Show N lines before |
| `-C N` | Show N lines context |

### Regex Metacharacters

| Character | Meaning |
|-----------|---------|
| `.` | Any single character |
| `*` | Zero or more |
| `+` | One or more |
| `?` | Zero or one |
| `^` | Start of line |
| `$` | End of line |
| `[]` | Character class |
| `\|` | OR operator (egrep) |
| `()` | Grouping (egrep) |
| `{n,m}` | Repeat n to m times |

## See Also

- [File Manipulation](file-manipulation.md) - Advanced text processing
- [Basic Commands](basic-commands.md) - File operations
- [Bash Scripting](bash-scripting.md) - Automation examples
