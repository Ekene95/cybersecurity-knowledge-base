# Caesar Cipher & TR Command

Text transformation and character manipulation using the `tr` (translate) command for encryption, decryption, and text processing.

## Caesar Cipher Overview

> In cryptography, a Caesar cipher, also known as Caesar's cipher, the shift cipher, Caesar's code or Caesar shift, is one of the simplest and most widely known encryption techniques. It is a type of substitution cipher in which each letter in the plaintext is replaced by a letter some fixed number of positions down the alphabet. For example, with a left shift of 3, D would be replaced by A, E would become B, and so on. The method is named after Julius Caesar, who used it in his private correspondence.

Source: [Wikipedia - Caesar Cipher](https://en.wikipedia.org/wiki/Caesar_cipher)

## Basic TR Usage

### Case Conversion

```bash
# Convert to uppercase
echo lInuX | tr [:lower:] [:upper:]
# Output: LINUX

# Convert to lowercase
echo LINUX | tr [:upper:] [:lower:]
# Output: linux
```

### Simple Caesar Cipher

```bash
# Decrypt Caesar cipher
echo EADGT UGEWTKVA KU CNUQ PKEG | tr '[C-ZA-B]' '[A-Z]'
```

## ROT-N Cipher Patterns

### All ROT Variations (1-25)

```bash
# ROT-3
echo "ENCRYPTED" | tr 'd-za-cD-ZA-C' 'a-zA-Z'

# ROT-4
echo "ENCRYPTED" | tr 'e-za-dE-ZA-D' 'a-zA-Z'

# ROT-5
echo "ENCRYPTED" | tr 'f-za-eF-ZA-E' 'a-zA-Z'

# ROT-6
echo "ENCRYPTED" | tr 'g-za-fG-ZA-F' 'a-zA-Z'

# ROT-7
echo "ENCRYPTED" | tr 'h-za-gH-ZA-G' 'a-zA-Z'

# ROT-8
echo "ENCRYPTED" | tr 'i-za-hI-ZA-H' 'a-zA-Z'

# ROT-9
echo "ENCRYPTED" | tr 'j-za-iJ-ZA-I' 'a-zA-Z'

# ROT-10
echo "ENCRYPTED" | tr 'k-za-jK-ZA-J' 'a-zA-Z'

# ROT-11
echo "ENCRYPTED" | tr 'l-za-kL-ZA-K' 'a-zA-Z'

# ROT-12
echo "ENCRYPTED" | tr 'm-za-lM-ZA-L' 'a-zA-Z'

# ROT-13 (Most common)
echo "ENCRYPTED" | tr 'n-za-mN-ZA-M' 'a-zA-Z'

# ROT-14
echo "ENCRYPTED" | tr 'o-za-nO-ZA-N' 'a-zA-Z'

# ROT-15
echo "ENCRYPTED" | tr 'p-za-oP-ZA-O' 'a-zA-Z'

# ROT-16
echo "ENCRYPTED" | tr 'q-za-pQ-ZA-P' 'a-zA-Z'

# ROT-17
echo "ENCRYPTED" | tr 'r-za-qR-ZA-Q' 'a-zA-Z'

# ROT-18
echo "ENCRYPTED" | tr 's-za-rS-ZA-R' 'a-zA-Z'

# ROT-19
echo "ENCRYPTED" | tr 't-za-sT-ZA-S' 'a-zA-Z'

# ROT-20
echo "ENCRYPTED" | tr 'u-za-tU-ZA-T' 'a-zA-Z'

# ROT-21
echo "ENCRYPTED" | tr 'v-za-uV-ZA-U' 'a-zA-Z'

# ROT-22
echo "ENCRYPTED" | tr 'w-za-vW-ZA-V' 'a-zA-Z'

# ROT-23
echo "ENCRYPTED" | tr 'x-za-wX-ZA-W' 'a-zA-Z'

# ROT-24
echo "ENCRYPTED" | tr 'y-za-xY-ZA-X' 'a-zA-Z'

# ROT-25
echo "ENCRYPTED" | tr 'z-za-yZ-ZA-Y' 'a-zA-Z'
```

Source: [AskUbuntu - Changing individual letter position with bash](https://askubuntu.com/questions/1097761/changing-individual-letter-position-with-bash)

## Advanced TR Operations

### Remove All Except Digits

```bash
# Extract only numbers from file
head -n 100 hackers.txt | tr -cd [:digit:]

# Alternative syntax
head -n 100 hackers.txt | tr -cd '0-9'
```

### Count Character Occurrences

```bash
# Count how many times 'A' appears
tr -cd 'A' < hackers.txt | wc -c
# Output: 959

tr -cd 'A' < hackers.txt | wc -m
# Output: 959 (same for ASCII)

# Count 'A' in first 1000 lines
head -n 1000 hackers.txt | tr -cd 'A' | wc -m
# Output: 197
```

### Delete Specific Characters

```bash
# Delete all digits
echo "abc123def456" | tr -d '0-9'
# Output: abcdef

# Delete all punctuation
echo "Hello, World!" | tr -d '[:punct:]'
# Output: Hello World

# Delete specific characters
echo "remove-these-dashes" | tr -d '-'
# Output: removethesedashes
```

### Squeeze Repeated Characters

```bash
# Squeeze repeated spaces to single space
echo "too    many     spaces" | tr -s ' '
# Output: too many spaces

# Squeeze repeated newlines
cat file.txt | tr -s '\n'

# Squeeze any whitespace
echo "text  \t\n  more" | tr -s '[:space:]'
```

## Character replacement Classes

### POSIX Character Classes

| Class | Description |
|-------|-------------|
| `[:alnum:]` | Alphanumeric characters |
| `[:alpha:]` | Alphabetic characters |
| `[:digit:]` | Digits (0-9) |
| `[:lower:]` | Lowercase letters |
| `[:upper:]` | Uppercase letters |
| `[:space:]` | Whitespace (space, tab, newline) |
| `[:punct:]` | Punctuation |
| `[:print:]` | Printable characters |
| `[:cntrl:]` | Control characters |
| `[:xdigit:]` | Hexadecimal digits |

## Practical Examples

### Data Cleaning

```bash
# Replace commas with tabs (CSV to TSV)
cat data.csv | tr ',' '\t' > data.tsv

# Remove Windows line endings (CRLF to LF)
cat windows_file.txt | tr -d '\r' > unix_file.txt

# Remove all special characters
cat file.txt | tr -cd '[:alnum:][:space:]'
```

### Password/Hash Analysis

```bash
# Extract only hex characters
cat hashes.txt | tr -cd '[:xdigit:]\n'

# Convert hash to uppercase
cat hash.txt | tr [:lower:] [:upper:]
```

### Log Analysis from OneNote Examples

```bash
# Practical examples from notes:
cat 82 | cut -d: -f2 | sort | uniq -c | sort -n
cat 82 | cut -d: -f1 | sort | uniq -c | sort -n

# Decrypt messages
echo "LWUMABQK IKP BZIVANMZA ILI ZWCBQVO" | tr [H-ZA-G] [A-Z]
echo "LWUMABQK IKP BZIVANMZA ILI ZWCBQVO" | tr [I-ZA-H] [A-Z]
echo "IKKWCVB VCULMZ" | tr [I-ZA-H] [A-Z]

# More decryption examples
echo "VWDI XTCULQVO & PMIBQVO KW., QVK" | tr [I-ZA-H] [A-Z]
echo "30 UQTTMZ IDMVCM" | tr [I-ZA-H] [A-Z]
echo "JIVS VIUM: JIVS WN IUMZQKI" | tr [I-ZA-H] [A-Z]
# Output: BANK NAME: BANK OF AMERICA
```

## Cipher Cracking Script

```bash
#!/bin/bash
# Try all ROT variations on encrypted text

ENCRYPTED="$1"

if [ -z "$ENCRYPTED" ]; then
    echo "Usage: $0 'ENCRYPTED TEXT'"
    exit 1
fi

echo "Trying all ROT ciphers..."
echo "========================="

for i in {1..25}; do
    # Calculate the shift
    char1=$(printf "\\$(printf '%03o' $((97+i)))")
    echo "ROT-$i: $(echo "$ENCRYPTED" | tr "[a-z]" "[$char1-za-$(printf "\\$(printf '%03o' $((96+i))")]" | tr "[A-Z]" "[$(echo $char1 | tr '[:lower:]' '[:upper:]')-ZA-$(printf "\\$(printf '%03o' $((64+i)))")]")"
done
```

## Combining TR with Other Commands

### With Cut

```bash
# Extract and transform
cat  leekpeek_extract.txt | awk '{print $1}' | cut -d: -f1 | tr [:upper:] [:lower:]
```

### With Grep

```bash
# Find and transform
grep "pattern" file.txt | tr -s ' ' | cut -d' ' -f2
```

### With Sed

```bash
# Complex transformation
cat file.txt | tr [:upper:] [:lower:] | sed 's/pattern/replacement/g'
```

## Performance Tips

1. **TR is faster than sed** for simple character substitutions
2. **Use `-c` for complementing** large character sets
3. **Combine with `-s`** to squeeze repeated characters efficiently
4. **Use character classes** instead of listing characters

## Quick Reference

### Common TR Operations

| Operation | Command |
|-----------|---------|
| Lowercase to uppercase | `tr [:lower:] [:upper:]` |
| Uppercase to lowercase | `tr [:upper:] [:lower:]` |
| Delete digits | `tr -d [:digit:]` |
| Keep only digits | `tr -cd [:digit:]` |
| Squeeze spaces | `tr -s ' '` |
| ROT13 | `tr 'n-za-mN-ZA-M' 'a-zA-Z'` |
| Remove newlines | `tr -d '\n'` |
| Replace chars | `tr 'abc' 'ABC'` |

## See Also

- [File Manipulation](file-manipulation.md) - Text processing with awk, sed
- [Encryption](encryption.md) - Strong encryption methods
- [Bash Scripting](bash-scripting.md) - Automation examples
