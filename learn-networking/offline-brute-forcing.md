# Offline Brute Forcing

Master password hash cracking with John the Ripper and wordlist generation - for **authorized security testing only**.

---

## ⚠️ **CRITICAL LEGAL WARNING**

**This guide is for AUTHORIZED TESTING ONLY!**

✅ **Legal Use:**
- Cracking your own password hashes
- Authorized penetration testing with written permission
- Educational CTF platforms and lab environments
- Security research on test systems

❌ **ILLEGAL Activities:**
- Cracking hashes obtained illegally
- Unauthorized access to accounts
- Cracking hashes without permission

**Possessing stolen credentials or cracking unauthorized hashes is a CRIME!**

---

## 📚 Table of Contents

- [Offline vs Online Attacks](#offline-vs-online-attacks)
- [John the Ripper](#john-the-ripper)
- [Hash Types](#hash-types)
- [Wordlist Generation](#wordlist-generation)
- [Advanced Techniques](#advanced-techniques)

---

## Offline vs Online Attacks

### Online Brute Force
- Tries passwords against live service
- Slow (rate limited)
- Detectable (shows in logs)
- Example: Hydra against SSH

### Offline Brute Force
- Cracks password hashes locally
- Fast (millions of attempts/second)
- Undetectable (no network traffic)
- Example: John the Ripper on `/etc/shadow`

---

## John the Ripper

**John the Ripper** (John) is a powerful offline password cracker.

### Installation

```bash
# Debian/Ubuntu/Kali (pre-installed on Kali)
sudo apt install john

# Check version
john --version
```

### Basic Usage

```bash
# Crack with default wordlist
john hashes.txt

# Specify hash format
john hashes.txt --format=crypt

# Use custom wordlist
john hashes.txt --wordlist=/usr/share/wordlists/rockyou.txt

# Show cracked passwords
john --show hashes.txt
```

---

## Hash Types

### Common Hash Formats

| Format | Example | Usage |
|--------|---------|-------|
| MD5 | `5f4dcc3b5aa765d61d8327deb882cf99` | Legacy (insecure) |
| SHA-1 | `5baa61e4c9b93f3f068...` | Legacy (insecure) |
| SHA-256 | `5e884898da28047151...` | Modern  |
| bcrypt | `$2y$10$...` | Strong (recommended) |
| NTLM | Windows hashes | Active Directory |
| Unix crypt | `$6$...` | /etc/shadow |

### Identifying Hash Types

```bash
# Use hash-identifier
hash-identifier

# Or hashid
hashid '5f4dcc3b5aa765d61d8327deb882cf99'
```

---

## Cracking Linux Passwords

### Extract Hashes from /etc/shadow

**Format of /etc/shadow:**
```
username:$6$salt$hash:lastchange:min:max:warn:inactive:expire:reserved
```

**Example:**
```
demo_user:$6$abc123$5FPl8q9jHuQKXs0oR...:18000:0:99999:7:::
```

### Crack with John

```bash
# Copy /etc/shadow (requires root)
sudo cp /etc/shadow shadow_backup.txt

# Crack with default wordlist
john shadow_backup.txt --format=crypt

# Use RockYou wordlist
john shadow_backup.txt --format=crypt --wordlist=/usr/share/wordlists/rockyou.txt

# Show results
john --show shadow_backup.txt
```

**Example Output:**
```
demo_user:password123:1000:1000:Demo User:/home/demo:/bin/bash

1 password hash cracked, 0 left
```

---

## Protected Files

### ZIP Files

Extract hash and crack:

```bash
# Convert ZIP to John format
zip2john protected_file.zip > zip_hash.txt

# Crack the hash
john zip_hash.txt --wordlist=/usr/share/wordlists/rockyou.txt

# Show password
john --show zip_hash.txt
```

### RAR Files

```bash
# Convert RAR to John format
rar2john protected_file.rar > rar_hash.txt

# Crack with wordlist
john rar_hash.txt --wordlist=/usr/share/wordlists/rockyou.txt

# For RAR5 format
john rar_hash.txt --format=RAR5 --wordlist=/usr/share/wordlists/rockyou.txt
```

### PDF Files

```bash
# Convert PDF to John format
pdf2john protected_document.pdf > pdf_hash.txt

# Crack the password
john pdf_hash.txt --format=PDF --wordlist=/usr/share/wordlists/rockyou.txt

# Show result
john --show pdf_hash.txt
```

### SSH Private Keys

```bash
# Convert SSH key to John format
ssh2john id_rsa > ssh_hash.txt

# Crack passphrase
john ssh_hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
```

---

## Wordlist Generation

### Crunch

**Crunch** generates custom wordlists based on patterns.

#### Installation

```bash
sudo apt install crunch
```

#### Basic Usage

```bash
# Generate all 4-digit passwords
crunch 4 4 0123456789

# Syntax:
# crunch [min_length] [max_length] [character_set]
```

#### Save to File

```bash
# Generate and save
crunch 4 4 0123456789 -o passwords.txt

# Example: All 4-character combinations of abc123
crunch 4 4 abc123 -o wordlist.txt

# Count without generating:
crunch 4 4 0123456789 -c
```

#### Using Templates

**Template Syntax:**
- `@` - Lowercase letters (a-z)
- `,` - Uppercase letters (A-Z)
- `%` - Numbers (0-9)
- `^` - Symbols (!@#$...)

```bash
# Phone numbers: 080-XXXXXXXX
crunch 10 10 -t 080%%%%%%%%

# Pattern: DogXXXX (where X = any char)
crunch 10 10 -t dog^@%,

# Pattern: Admin2024!
crunch 10 10 -t Admin%%%%^
```

**Complete Examples:**
```bash
# Generate all variations of "password" + 2 digits
crunch 10 10 -t password%%

# Mobile numbers starting with 080
crunch 13 13 -t 080%%%%%%%%%%

# Email format variations
crunch 8 8 -t user%%%%
```

---

## CUPP - Custom Wordlists

**CUPP** (Common User Password Profiler) generates targeted wordlists based on target  information.

### Installation

```bash
# Clone from GitHub
git clone https://github.com/Mebus/cupp.git
cd cupp
```

### Interactive Mode

```bash
# Run interactive mode
python3 cupp.py -i

# It will ask questions:
# - First name
# - Last name
# - Nickname
# - Birthdate
# - Partner's name
# - Pet's name
# - Company name
# - Etc.

# Generates customized wordlist
```

**Example:**
```
First Name: John
Surname: Smith
Nickname: johnny
Birthdate: 15081990
Partner name: Sarah
Children's name: Emma
Pet's name: Max

Output: john_wordlist.txt (thousands of variations)
Examples:
- John1990
- johnsmith
- johnnyEmma
- SmithMax
- Sarah1990
- john150890
```

---

## Advanced Techniques

### Incremental Mode

Try all possible combinations (brute force):

```bash
# Incremental mode (slow but thorough)
john hashes.txt --incremental

# Specify character set
john hashes.txt --incremental=Digits     # 0-9 only
john hashes.txt --incremental=Alpha      # a-zA-Z
john hashes.txt --incremental=Alnum      # a-zA-Z0-9
```

### Rules

**John rules** modify wordlist entries:

```bash
# Use built-in rules
john hashes.txt --wordlist=passwords.txt --rules

# Common transformations:
# password → Password
# password → password1
# password → p@ssword
# password → drowssap (reversed)
```

### Combining Wordlists

```bash
# Merge multiple wordlists
cat wordlist1.txt wordlist2.txt wordlist3.txt > combined.txt

# Remove duplicates
sort combined.txt | uniq > unique_passwords.txt

# Merge and sort
cat *.txt | sort -u > master_wordlist.txt
```

---

## Hashcat (Alternative to John)

**Hashcat** is faster (GPU-accelerated) than John.

### Basic Hashcat

```bash
# Install
sudo apt install hashcat

# MD5 hash
hashcat -m 0 -a 0 hash.txt wordlist.txt

# SHA-256
hashcat -m 1400 -a 0 hash.txt wordlist.txt

# NTLM (Windows)
hashcat -m 1000 -a 0 hash.txt wordlist.txt

# Options:
# -m: hash mode
# -a: attack mode (0=dictionary)
```

---

## Real-World Scenario

**CTF Challenge: Crack Protected ZIP**

```bash
# Step 1: Extract hash from ZIP
zip2john flag.zip > flag_hash.txt

# Step 2: Try RockYou wordlist
john flag_hash.txt --wordlist=/usr/share/wordlists/rockyou.txt

# Step 3: If that fails, generate custom wordlist
# Based on challenge hints (e.g., year 2024, name=admin)
crunch 10 10 -t admin%%%% > custom.txt

# Step 4: Try custom wordlist
john flag_hash.txt --wordlist=custom.txt

# Step 5: Show password
john --show flag_hash.txt

# Step 6: Unzip file
unzip flag.zip
# Enter the cracked password
```

---

## Defense Against Hash Cracking

### 1. Use Strong Hashing

```bash
# ❌ Weak: MD5, SHA-1
echo -n "password" | md5sum

# ✅ Good: bcrypt, scrypt, Argon2
# These are intentionally slow!
```

### 2. Salt Hashes

```bash
# Salting adds random data before hashing
# Makes rainbow table attacks ineffective

# Without salt:
# "password" → 5f4dcc3b5aa765d61d8327deb882cf99

# With salt:
# "password" + "randomsalt123" → different hash every time
```

### 3. Increase Iterations

```bash
# More iterations = slower cracking
# Configure in /etc/login.defs (Linux)

# Example: bcrypt with cost factor 12
# Higher cost = slower but more secure
```

### 4. Password Policies

```bash
# Enforce:
# - Minimum length (12+ characters)
# - Complexity requirements
# - No common passwords
# - Regular password changes (debatable)
```

---

## Practice Exercises

1. Create custom wordlist with crunch for 6-digit PINs
2. Use CUPP to generate wordlist for fictional person
3. Crack a practice ZIP file hash
4. Generate wordlist matching pattern: Admin20XX (years 2000-2024)
5. Compare John vs Hashcat speed

---

## Quick Reference

### John the Ripper

```bash
# Basic crack
john hashes.txt

# With wordlist
john hashes.txt --wordlist=passwords.txt

# Show cracked
john --show hashes.txt

# Specific format
john hashes.txt --format=MD5
```

### File to Hash

```bash
zip2john file.zip > hash.txt
rar2john file.rar > hash.txt
pdf2john file.pdf > hash.txt
ssh2john id_rsa > hash.txt
```

### Crunch Templates

```bash
@ = lowercase
, = uppercase
% = numbers
^ = symbols

# Examples:
crunch 8 8 -t admin%%%
crunch 10 10 -t 080%%%%%%%
```

---

## See Also

- [Online Brute Forcing](online-brute-forcing.md) - Live service attacks
- [Text Manipulation](text-manipulation.md) - Process wordlists
- [Network Services](network-services.md) - Services that use password hashes
