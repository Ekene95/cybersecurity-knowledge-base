# Steganography

Hidden data detection and extraction - find secret messages hidden in files.

---

## 📚 Table of Contents

- [What is Steganography?](#what-is-steganography)
- [Common Tools](#common-tools)
- [Image Steganography](#image-steganography)
- [File Analysis](#file-analysis)
- [Data Extraction](#data-extraction)
- [CTF Techniques](#ctf-techniques)

---

## What is Steganography?

**Steganography** is the practice of hiding data within other data.

**Common Uses:**
- 🕵️ Hiding messages in images
- 📂 Concealing files within files  
- 🔐 Covert communication
- 🎯 CTF challenges

**Difference from Encryption:**
- **Encryption**: Everyone knows data exists, but can't read it
- **Steganography**: Nobody knows data exists at all

---

## Common Tools

### Installation

```bash
# Install essential stego tools
sudo apt update
sudo apt install steghide binwalk exiftool foremost strings

# Additional tools
sudo apt install stegsolve outguess zsteg
```

### Tool Overview

| Tool | Purpose |
|------|---------|
| **steghide** | Hide/extract data in images |
| **binwalk** | Analyze and extract embedded files |
| **exiftool** | Read/write image metadata |
| **strings** | Extract readable text from files |
| **foremost** | Carve files from binary data |
| **zsteg** | Detect LSB steganography in PNG/BMP |

---

## Image Steganography

### steghide

**Hide Data:**
```bash
# Hide secret.txt inside image.jpg
steghide embed -cf image.jpg -ef secret.txt

# With password
steghide embed -cf image.jpg -ef secret.txt -p MySecretPass123

# -cf: cover file (image)
# -ef: embed file (data to hide)
# -p: passphrase
```

**Extract Data:**
```bash
# Extract hidden data
steghide extract -sf image.jpg

# With password
steghide extract -sf image.jpg -p MySecretPass123

# Get info about hidden data (without password)
steghide info image.jpg
```

**Example:**
```bash
# Create secret message
echo "The password is: flag{hidden_message}" > secret.txt

# Hide in image
steghide embed -cf photo.jpg -ef secret.txt -p infected

# Extract later
steghide extract -sf photo.jpg -p infected
# Output: extracted secret.txt
```

---

## File Analysis

### strings

Extract readable text from any file:

```bash
# Basic usage
strings suspicious_file.jpg

# Look for specific patterns
strings image.jpg | grep -i "flag"
strings file.bin | grep -i "password"

# Minimum string length
strings -n 10 file.dat    # Strings ≥ 10 characters
```

### exiftool

Check image metadata:

```bash
# View all metadata
exiftool image.jpg

# Look for specific fields
exiftool -Comment image.jpg
exiftool -Artist image.jpg

# Check all images in directory
exiftool *.jpg

# Export to CSV
exiftool -csv *.jpg > metadata.csv
```

**Common Hidden Locations:**
- Comment field
- Artist/Author field
- Description  
- GPS coordinates
- Custom fields

---

## binwalk

**binwalk** finds embedded files and executables.

### Basic Analysis

```bash
# Analyze file for embedded data
binwalk suspicious_file.jpg

# Verbose output
binwalk -v file.bin

# Look for signatures
binwalk --signature file.dat
```

**Example Output:**
```
DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data
54272         0xD400          Zip archive data
```

### Extract Embedded Files

```bash
# Extract all found files
binwalk -e suspicious_file.jpg

# Creates directory: _suspicious_file.jpg.extracted/

# Extract specific type
binwalk --dd='zip' suspicious_file.jpg

# -e: extract
# --dd: extract specific signature
```

### Real Example

```bash
# Download sample file
wget https://example.com/suspicious_image.png

# Analyze
binwalk suspicious_image.png

# If ZIP found at offset 54272:
# Extract manually
dd if=suspicious_image.png of=extracted.zip bs=1 skip=54272

# Or auto-extract
binwalk -e suspicious_image.png

# Unzip extracted file
cd _suspicious_image.png.extracted/
unzip *.zip -p infected    # Password often: "infected"
```

---

## Archive Analysis

### 7zip with Password

If you find password-protected archive:

```bash
# Try extracting with common password
7z x filename.7z -pinfected

# Common CTF passwords:
# - infected
# - password
# - hackthebox
# - tryhackme
```

### ZIP Files

```bash
# Extract ZIP
unzip file.zip

# With password
unzip file.zip -P password123

# List contents without extracting
unzip -l file.zip

# Extract to specific directory
unzip file.zip -d extracted/
```

---

## Data Extraction Pipeline

### Complete Stego Workflow

```bash
#!/bin/bash
# stego_analyze.sh - Automated steganography analysis

FILE=$1

echo "[+] Analyzing: $FILE"

# 1. Check file type
echo "[1] File type:"
file $FILE

# 2. Extract readable strings
echo -e "\n[2] Searching for strings..."
strings $FILE | grep -E "(flag|password|key|secret)" > strings_output.txt
cat strings_output.txt

# 3. Check metadata
echo -e "\n[3] Metadata:"
exiftool $FILE | grep -E "(Comment|Artist|Description)"

# 4. Look for embedded files
echo -e "\n[4] Embedded files:"
binwalk $FILE

# 5. Try steghide (common CTF)
echo -e "\n[5] Trying steghide extraction..."
steghide extract -sf $FILE -p "" 2>/dev/null || echo "Password protected or no data"

echo -e "\n[+] Analysis complete!"
```

**Usage:**
```bash
chmod +x stego_analyze.sh
./stego_analyze.sh suspicious_image.jpg
```

---

## LSB Steganography

**LSB (Least Significant Bit)** hides data in the least important bits of pixels.

### zsteg (PNG/BMP)

```bash
# Analyze PNG/BMP for LSB stego
zsteg image.png

# All possible extractions
zsteg --all image.png

# Extract specific channel
zsteg -E "b1,rgb,lsb" image.png > extracted.txt

# Common options:
# b1 = LSB
# rgb = color channels
# lsb = least significant bit
```

### stegsolve (Java tool)

```bash
# Install
wget http://www.caesum.com/handbook/Stegsolve.jar

# Run
java -jar Stegsolve.jar

# GUI Interface:
# 1. Open image
# 2. Use arrow keys to cycle through bit planes
# 3. Look for hidden patterns
# 4. Analyze → Data Extract
```

---

## CTF Techniques

### Common CTF Stego Patterns

1. **Password: "infected"**
   ```bash
   steghide extract -sf image.jpg -p infected
   7z x file.7z -pinfected
   ```

2. **Hidden Zips in Images**
   ```bash
   binwalk -e image.png
   unzip *.zip -p infected
   ```

3. **Metadata Comments**
   ```bash
   exiftool image.jpg | grep Comment
   ```

4. **Base64 in Strings**
   ```bash
   strings image.jpg | grep -E '^[A-Za-z0-9+/=]{20,}$' | base64 -d
   ```

5. **ROT13 Encoded**
   ```bash
   strings image.jpg | tr 'A-Za-z' 'N-ZA-Mn-za-m'
   ```

---

## Advanced: Database Extraction

From your notes - analyzing leaked databases:

```bash
# Example: Extract specific domain from database dump
cat leaked_database.db | grep -i "example.com"

# Extract just usernames
cat database.db | awk -F'@' '{print $1}'

# Extract and count by domain
cat database.db | grep -i "example.com" | awk -F'@' '{print $1}' | wc -l

# Full extraction pipeline
cat hacked.db | \
  grep -i "cia.gov" | \
  awk -F'@' '{print $1}' | \
  awk '{print $2}' | \
  sort | \
  uniq -c | \
  wc -l
```

---

## Real CTF Example

**Challenge:** Find flag in image.jpg

```bash
# Step 1: Check file type
file image.jpg
# Output: JPEG image data

# Step 2: Look for strings
strings image.jpg | grep flag
# Output: Nothing

# Step 3: Check metadata
exiftool image.jpg
# Output: Comment: "Password is: mysecret"

# Step 4: Try steghide with found password
steghide extract -sf image.jpg -p mysecret
# Output: wrote extracted data to "flag.txt"

# Step 5: Read flag
cat flag.txt
# Output: flag{st3g0_m4st3r_2024}
```

---

## Wine for Windows Tools

Some stego tools are Windows-only. Use **wine**:

```bash
# Install wine
sudo apt install wine

# Run Windows .exe files
wine malware_sample.exe    # ONLY in isolated VM!

# Example: Run Windows stego tool
wine StegTool.exe
```

**⚠️ Security Warning:** Only run untrusted .exe files in isolated VMs!

---

## Automated Tools

### StegCracker

Brute force steghide passwords:

```bash
# Install
pip3 install stegcracker

# Usage
stegcracker image.jpg wordlist.txt

# With RockYou
stegcracker image.jpg /usr/share/wordlists/rockyou.txt
```

### stegseek (Faster StegCracker)

```bash
# Install
sudo apt install stegseek

# Usage (much faster!)
stegseek image.jpg /usr/share/wordlists/rockyou.txt
```

---

## Defense: Detecting Steganography

### As a Defender

```bash
# Check for anomalies
binwalk suspicious_files/*

# Batch check metadata
exiftool -r /path/to/images/ > metadata_audit.txt

# Look for unusual file sizes
find . -name "*.jpg" -size +5M    # JPEGs > 5MB unusual

# Calculate entropy (high entropy = possible encryption/stego)
ent suspicious_file.jpg
```

---

## Practice Resources

**Legal Practice:**
1. **picoCTF** - https://picoctf.org/
2. **CTFtime** - https://ctftime.org/
3. **OverTheWire** - Stego challenges
4. **HackTheBox** - Forensics challenges

---

## Quick Reference

### Essential Commands

```bash
# Extract readable text
strings file.jpg | grep flag

# Check metadata
exiftool file.jpg

# Find embedded files
binwalk file.jpg
binwalk -e file.jpg    # Extract

# Steghide extract
steghide extract -sf file.jpg -p password

# LSB analysis (PNG)
zsteg file.png
```

### Common Passwords

```
infected
password
hackthebox
tryhackme
admin
secret
```

---

## See Also

- [Text Manipulation](text-manipulation.md) - Process extracted data
- [Offline Brute Forcing](offline-brute-forcing.md) - Crack stego passwords
- [NMAP Scanning](nmap-scanning.md) - Find systems to analyze
