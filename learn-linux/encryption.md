# Encryption & Decryption

Data encryption and decryption utilities using OpenSSL and other Linux tools.

## OpenSSL Encryption

### Support Dump Tool Encryption

```bash
# Encrypt with password protection
/opt/kubernetes/tools/support-tool/support-dump -u admin -p passwd -P qwertasdf -c /tmp/dump
```

**Note:** Replace `passwd` with the actual admin user cluster password.

### Decrypting Archive Files

#### AES-256-CBC (Recommended)

```bash
# Decrypt with AES-256-CBC and extract
dd if=encrypted_file | openssl aes-256-cbc -md sha1 -d -k PASSWORD | tar zxf -
```

**Flags explained:**
- `dd if=encrypted_file` - Read the encrypted file
- `openssl aes-256-cbc` - Use AES-256-CBC encryption
- `-md sha1` - Use SHA1 message digest
- `-d` - Decrypt mode
- `-k PASSWORD` - Password for decryption
- `tar zxf -` - Extract gzipped tar from stdin

#### AES-256-ECB (Less Secure)

```bash
# Decrypt with AES-256-ECB
dd if=encrypted_file | openssl aes-256-ecb -d -k PASSWORD | tar zxf -
```

**Note:** ECB mode is less secure than CBC. Use CBC when possible.

## OpenSSL Basics

### File Encryption

```bash
# Encrypt a file with AES-256-CBC
openssl aes-256-cbc -salt -in file.txt -out file.txt.enc

# Decrypt a file
openssl aes-256-cbc -d -in file.txt.enc -out file.txt

# Use password file
openssl aes-256-cbc -in file.txt -out file.txt.enc -pass file:password.txt

# Use environment variable
openssl aes-256-cbc -in file.txt -out file.txt.enc -pass env:MY_PASSWORD
```

### Base64 Encoding/Decoding

```bash
# Encode file to base64
openssl base64 -in file.bin -out file.b64

# Decode from base64
openssl base64 -d -in file.b64 -out file.bin

# Pipe encoding
cat file.txt | openssl base64

# Pipe decoding
cat file.b64 | openssl base64 -d
```

## GPG Encryption

### Symmetric Encryption

```bash
# Encrypt with password
gpg -c file.txt
# Creates: file.txt.gpg

# Decrypt
gpg file.txt.gpg
# or
gpg -d file.txt.gpg > file.txt

# Specify cipher algorithm
gpg --cipher-algo AES256 -c file.txt
```

### Public Key Encryption

```bash
# Generate key pair
gpg --gen-key

# List keys
gpg --list-keys

# Export public key
gpg --export -a "User Name" > public.key

# Import public key
gpg --import public.key

# Encrypt for recipient
gpg -e -r "Recipient Name" file.txt

# Decrypt
gpg -d file.txt.gpg > file.txt

# Sign and encrypt
gpg -se -r "Recipient Name" file.txt
```

## Archive Encryption Workflows

### Encrypt Before Archiving

```bash
# Create encrypted tar archive
tar czf - /path/to/data | openssl aes-256-cbc -out archive.tar.gz.enc

# Decrypt and extract
openssl aes-256-cbc -d -in archive.tar.gz.enc | tar xzf -
```

### Archive Then Encrypt

```bash
# Create archive first
tar czf archive.tar.gz /path/to/data

# Then encrypt
openssl aes-256-cbc -in archive.tar.gz -out archive.tar.gz.enc

# Decrypt
openssl aes-256-cbc -d -in archive.tar.gz.enc -out archive.tar.gz

# Extract
tar xzf archive.tar.gz
```

## Password Management

### Generate Strong Passwords

```bash
# Random password (32 chars)
openssl rand -base64 32

# Hex password
openssl rand -hex 16

# Alphanumeric password
tr -dc 'A-Za-z0-9' < /dev/urandom | head -c 20
```

### Hash Passwords

```bash
# SHA-256 hash
echo -n "password" | openssl dgst -sha256

# MD5 hash (weak, avoid for security)
echo -n "password" | openssl dgst -md5

# bcrypt-style hash
htpasswd -nbB user password
```

## Verifying File Integrity

### Create Checksums

```bash
# MD5 checksum
md5sum file.txt > file.txt.md5

# SHA-256 checksum
sha256sum file.txt > file.txt.sha256

# Multiple files
md5sum * > checksums.md5
```

### Verify Checksums

```bash
# Verify MD5
md5sum -c file.txt.md5

# Verify SHA-256
sha256sum -c file.txt.sha256

# Verify all files in directory
md5sum -c checksums.md5
```

## SSL/TLS Certificates

### Generate Self-Signed Certificate

```bash
# Generate private key and certificate
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes

# With specific subject
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes \
  -subj "/C=US/ST=State/L=City/O=Organization/CN=example.com"
```

### View Certificate Information

```bash
# View certificate details
openssl x509 -in cert.pem -text -noout

# Check certificate expiry  
openssl x509 -in cert.pem -noout -enddate

# Verify certificate chain
openssl verify -CAfile ca-cert.pem cert.pem
```

### Test SSL Connections

```bash
# Test HTTPS connection
openssl s_client -connect example.com:443

# Show certificate
openssl s_client -connect example.com:443 -showcerts

# Test with SNI
openssl s_client -connect example.com:443 -servername example.com
```

## Encryption Best Practices

### Algorithm Selection

| Algorithm | Use Case | Security Level |
|-----------|----------|----------------|
| AES-256-CBC | General file encryption | High |
| AES-256-GCM | Authenticated encryption | Very High |
| ChaCha20-Poly1305 | Mobile/embedded | Very High |
| RSA-4096 | Key exchange | High |
| Ed25519 | SSH keys, signing | Very High |

### Key Management

1. **Never hardcode passwords** - Use environment variables or key files
2. **Use strong passwords** - At least 20 characters, random
3. **Rotate keys regularly** - At least annually
4. **Secure key storage** - Use proper permissions (600)
5. **Backup encrypted data** - Store keys separately from data

### File Permissions

```bash
# Secure private key
chmod 600 private.key

# Secure password file
chmod 400 password.txt

# Secure encrypted files
chmod 600 encrypted.enc
```

## Practical Examples

### Encrypted Backup Script

```bash
#!/bin/bash
BACKUP_DIR="/data"
BACKUP_FILE="/backup/backup-$(date +%Y%m%d).tar.gz.enc"
PASSWORD="$(cat /secure/backup.key)"

# Create encrypted backup
tar czf - "$BACKUP_DIR" | openssl aes-256-cbc -k "$PASSWORD" -out "$BACKUP_FILE"

# Verify encryption
if [ -f "$BACKUP_FILE" ]; then
    echo "Backup created: $BACKUP_FILE"
    ls -lh "$BACKUP_FILE"
else
    echo "Backup failed!"
    exit 1
fi
```

### Secure File Transfer

```bash
# Encrypt before transfer
openssl aes-256-cbc -in sensitive.doc -out sensitive.doc.enc

# Transfer encrypted file
scp sensitive.doc.enc user@remote:/path/

# On remote: decrypt
openssl aes-256-cbc -d -in sensitive.doc.enc -out sensitive.doc

# Clean up
rm sensitive.doc.enc
```

### Password-Protected Archive

```bash
# Create password-protected zip
zip -er archive.zip /path/to/files/

# Create password-protected 7z
7z a -p -mhe=on archive.7z /path/to/files/

# Extract 7z
7z x archive.7z
```

## Troubleshooting

### Common Errors

```bash
# "bad decrypt" error
# - Wrong password
# - Different encryption algorithm
# - Corrupted file

# "bad magic number" error
# - File not encrypted with expected algorithm
# - File corrupted

# "unknown option" error
# - Check OpenSSL version compatibility
# - Verify command syntax
```

## See Also

- [Archiving](archiving.md) - File compression and archives
- [Caesar Cipher](caesar-cipher.md) - Simple encryption/decryption
- [Useful Commands](useful-commands.md) - Base64 encoding examples
