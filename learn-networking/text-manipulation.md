# Text Manipulation for Security

Master command-line text processing - essential for analyzing logs, processing scan results, and extracting data.

---

## 📚 Table of Contents

- [Why Text Manipulation?](#why-text-manipulation)
- [grep - Search Text](#grep---search-text)
- [awk - Process Columns](#awk---process-columns)
- [sed - Stream Editor](#sed---stream-editor)
- [cut & sort](#cut--sort)
- [uniq & wc](#uniq--wc)
- [Security Use Cases](#security-use-cases)

---

## Why Text Manipulation?

In security work, you'll constantly process:
- 📋 Log files
- 🔍 Scan results
- 📊 Large datasets
- 🗂️ Lists of IPs, URLs, emails

**Command-line tools** let you:
- Filter millions of lines instantly
- Extract specific data
- Transform formats
- Automate repetitive tasks

---

## grep - Search Text

**grep** searches for patterns in text.

### Basic Usage

```bash
# Search for word in file
grep "error" logfile.txt

# Case-insensitive search
grep -i "error" logfile.txt

# Show line numbers
grep -n "error" logfile.txt

# Invert match (show lines NOT containing pattern)
grep -v "success" logfile.txt
```

### Recursive Search

```bash
# Search all files in directory
grep -r "password" /var/log/

# Search with specific file type
grep -r --include="*.log" "failed" /var/log/

# Exclude certain files
grep -r --exclude="*.tmp" "error" /var/log/
```

### Regular Expressions

```bash
# Find IP addresses
grep -E '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' logfile.txt

# Find email addresses
grep -E '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}' emails.txt

# Find lines starting with "Error"
grep '^Error' logfile.txt

# Find lines ending with "failed"
grep 'failed$' logfile.txt
```

### Useful Options

```bash
# -i: Case insensitive
grep -i "admin" users.txt

# -R or -r: Recursive search
grep -R "password" /etc/

# -n: Show line numbers
grep -n "error" app.log

# -c: Count matches
grep -c "failed" auth.log

# -l: Show only filenames
grep -l "error" *.log

# -A 3: Show 3 lines after match
grep -A 3 "error" app.log

# -B 3: Show 3 lines before match
grep -B 3 "error" app.log

# -C 3: Show 3 lines before and after
grep -C 3 "error" app.log
```

---

## awk - Process Columns

**awk** is powerful for processing columnar data.

### Basic Syntax

```bash
# Print specific column
awk '{print $1}' file.txt        # First column
awk '{print $2}' file.txt        # Second column
awk '{print $1, $3}' file.txt    # First and third

# Print entire line
awk '{print $0}' file.txt
```

### Field Separator

```bash
# Default separator is whitespace
# Use -F to specify delimiter

# CSV file (comma-separated)
awk -F',' '{print $1}' data.csv

# Colon-separated (like /etc/passwd)
awk -F':' '{print $1}' /etc/passwd

# Multiple character delimiter
awk -F' - ' '{print $2}' logfile.txt

# Tab-separated
awk -F'\t' '{print $1}' file.tsv
```

### Filtering with awk

```bash
# Print lines where column 3 > 100
awk '$3 > 100' data.txt

# Print lines where column 1 equals "admin"
awk '$1 == "admin"' users.txt

# Print lines containing "error"
awk '/error/' logfile.txt

# Combine conditions
awk '$3 > 100 && $1 == "admin"' data.txt
```

### awk Examples

```bash
# Print usernames from /etc/passwd
awk -F':' '{print $1}' /etc/passwd

# Sum values in column 3
awk '{sum += $3} END {print sum}' numbers.txt

# Count lines
awk 'END {print NR}' file.txt

# Print lines number 10-20
awk 'NR>=10 && NR<=20' file.txt
```

---

## sed - Stream Editor

**sed** edits streams of text.

### Find and Replace

```bash
# Replace first occurrence on each line
sed 's/old/new/' file.txt

# Replace all occurrences (global)
sed 's/old/new/g' file.txt

# Replace in-place (modify file)
sed -i 's/old/new/g' file.txt

# Case-insensitive replace
sed 's/old/new/gi' file.txt
```

### Delete Lines

```bash
# Delete line 5
sed '5d' file.txt

# Delete lines 5-10
sed '5,10d' file.txt

# Delete lines containing "error"
sed '/error/d' file.txt

# Delete empty lines
sed '/^$/d' file.txt
```

### Extract Lines

```bash
# Print line 10
sed -n '10p' file.txt

# Print lines 10-20
sed -n '10,20p' file.txt

# Print lines matching "error"
sed -n '/error/p' file.txt
```

---

## cut & sort

### cut - Extract Columns

```bash
# Extract characters 1-10
cut -c 1-10 file.txt

# Extract fields (columns) delimited by space
cut -d' ' -f1 file.txt

# Extract multiple fields
cut -d',' -f1,3,5 data.csv

# Extract field range
cut -d':' -f1-3 /etc/passwd
```

### sort - Sort Lines

```bash
# Sort alphabetically
sort file.txt

# Sort numerically
sort -n numbers.txt

# Sort in reverse
sort -r file.txt

# Sort by second column
sort -k2 file.txt

# Sort uniquely (remove duplicates)
sort -u file.txt

# Sort by numeric column 3
sort -n -k3 data.txt
```

---

## uniq & wc

### uniq - Remove Duplicates

```bash
# Remove adjacent duplicate lines (must sort first!)
sort file.txt | uniq

# Count occurrences
sort file.txt | uniq -c

# Show only duplicates
sort file.txt | uniq -d

# Show only unique lines
sort file.txt | uniq -u
```

### wc - Word Count

```bash
# Count lines
wc -l file.txt

# Count words
wc -w file.txt

# Count characters
wc -c file.txt

# Count all
wc file.txt
```

---

## Security Use Cases

### 1. Extract IPs from Logs

```bash
# Find all IP addresses in access log
grep -oE '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' access.log

# Count unique IPs
grep -oE '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' access.log | sort | uniq -c | sort -rn

# Top 10 most frequent IPs
grep -oE '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' access.log | sort | uniq -c | sort -rn | head -10
```

### 2. Analyze Failed Login Attempts

```bash
# Find failed SSH logins
grep "Failed password" /var/log/auth.log

# Count failures per IP
grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -rn

# IPs with more than 5 failures
grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | awk '$1 > 5'
```

### 3. Process NMAP Output

```bash
# Extract open ports from NMAP gnmap file
grep "open" scan.gnmap | awk '{print $2, $4}' | sed 's/Ports://'

# Get unique open ports across all hosts
grep "open" scan.gnmap | grep -oE '[0-9]+/open' | cut -d'/' -f1 | sort -n | uniq
```

### 4. Extract Emails from Data

```bash
# Find all email addresses
grep -oE '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}' file.txt

# Get unique domains
grep -oE '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}' file.txt | awk -F'@' '{print $2}' | sort | uniq -c | sort -rn
```

### 5. Process Password List

```bash
# Remove duplicates
sort passwords.txt | uniq > unique_passwords.txt

# Count password length distribution
awk '{print length($0)}' passwords.txt | sort -n | uniq -c

# Filter passwords 8-16 characters
awk 'length($0) >= 8 && length($0) <= 16' passwords.txt
```

### 6. Analyze Web Server Logs

```bash
# Count requests per URL
awk '{print $7}' access.log | sort | uniq -c | sort -rn | head -20

# Find 404 errors
awk '$9 == 404 {print $7}' access.log | sort | uniq -c | sort -rn

# Top user agents
awk -F'"' '{print $6}' access.log | sort | uniq -c | sort -rn | head -10
```

### 7. Extract Subdomain from List

```bash
# Example: Extract subdomain from full URLs
echo "https://www.example.com/path" | awk -F'/' '{print $3}' | awk -F'.' '{print $1}'

# From list of URLs
cat urls.txt | awk -F'/' '{print $3}' | awk -F'.' '{print $1}' | sort | uniq
```

### 8. Process Database Dump

```bash
# Example database with pattern: email:password
# Extract just emails
cat database.txt | awk -F':' '{print $1}' > emails.txt

# Extract domain from emails
cat database.txt | awk -F':' '{print $1}' | awk -F'@' '{print $2}' | sort | uniq -c | sort -rn

# Count entries per domain
cat database.txt | grep -i "cia.gov" | awk -F'@' '{print $1}' | awk '{print $2}' | sort | uniq -c | wc -l
```

---

## Combining Commands (Pipes)

**Pipes** (`|`) chain commands together:

```bash
# Find, sort, count unique
grep "error" app.log | sort | uniq -c

# Extract IPs, sort numerically, show top 5
grep -oE '[0-9.]+' access.log | sort -t. -k1,1n -k2,2n -k3,3n -k4,4n | head -5

# Complex pipeline
cat access.log | \
  awk '{print $1}' | \      # Extract IP
  sort | \                   # Sort
  uniq -c | \                # Count occurrences
  sort -rn | \               # Sort by count (descending)
  head -10                  # Top 10
```

---

## Real-World Example

**Scenario:** Analyze failed login attempts and block suspicious IPs

```bash
#!/bin/bash
# analyze_failed_logins.sh

LOG_FILE="/var/log/auth.log"
THRESHOLD=5

echo "Analyzing failed SSH login attempts..."

# Extract IPs with failed logins
grep "Failed password" "$LOG_FILE" | \
  awk '{print $(NF-3)}' | \
  sort | \
  uniq -c | \
  sort -rn | \
  awk -v threshold="$THRESHOLD" '$1 >= threshold {print $2, "(" $1 " attempts)"}' | \
  while read ip attempts; do
    echo "Suspicious IP: $ip - $attempts"
    
    # Optionally: Add to firewall (commented for safety)
    # sudo ufw deny from $ip
done

echo "Analysis complete!"
```

---

## Practice Exercises

1. Extract all unique user-agents from web server access log
2. Find top 20 IPs with most failed login attempts
3. Count how many URLs return 404 errors
4. Extract all email addresses from a text file and group by domain
5. Process NMAP output to list all hosts with port 22 open

---

## Quick Reference

### Essential Patterns

```bash
# Search case-insensitive
grep -i "pattern" file

# Extract column 2
awk '{print $2}' file

# Replace all occurrences
sed 's/old/new/g' file

# Sort and remove duplicates
sort file | uniq

# Count lines
wc -l file

# Extract IPs
grep -oE '[0-9]{1,3}(\.[0-9]{1,3}){3}' file

# Pipe multiple commands
cat file | grep "error" | awk '{print $1}' | sort | uniq -c
```

---

## See Also

- [NMAP Scanning](nmap-scanning.md) - Process scan results
- [Wireshark Basics](wireshark-basics.md) - Analyze captured data  
- [Online Brute Forcing](online-brute-forcing.md) - Process wordlists
- [Learn Linux](../learn-linux/) - More Linux command practice
