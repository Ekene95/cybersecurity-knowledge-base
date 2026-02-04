# Strings, Variables & Data Types

Your first steps in Python - learn how to store and manipulate data for security scripts and automation.

---

## 📚 Table of Contents

- [Variables](#variables)
- [Strings](#strings)
- [String Methods](#string-methods)
- [String Formatting](#string-formatting)
- [Numbers](#numbers)
- [Type Conversion](#type-conversion)
- [Security Examples](#security-examples)

---

## Variables

Variables store data that you can use and manipulate in your programs.

### Creating Variables

```python
# Simple assignment
name = "Alice"
age = 25
is_admin = True
score = 95.5

# Multiple assignment
x, y, z = 10, 20, 30

# Same value to multiple variables
a = b = c = 0
```

### Variable Naming Rules

✅ **Valid:**
```python
username = "admin"
user_name = "admin"
userName = "admin"
user2 = "admin"
_private = "secret"
```

❌ **Invalid:**
```python
2user = "admin"      # Can't start with number
user-name = "admin"  # No hyphens
user name = "admin"  # No spaces
for = "admin"        # Can't use keywords
```

### Best Practices

```python
# Good - descriptive names
failed_login_attempts = 0
target_ip_address = "192.0.2.10"
api_key = "abc123xyz"

# Bad - unclear names
x = 0
ip = "192.0.2.10"
k = "abc123xyz"
```

---

## Strings

Strings are sequences of characters used for text data.

### Creating Strings

```python
# Single quotes
name = 'Alice'

# Double quotes
message = "Hello, World!"

# Triple quotes for multiline
log_entry = """
2024-01-15 10:30:45
User: admin
Action: login_failed
IP: 192.0.2.50
"""

# Empty string
empty = ""
```

### String Indexing

```python
text = "Python"

# Positive indexing (left to right)
print(text[0])   # 'P'
print(text[1])   # 'y'
print(text[5])   # 'n'

# Negative indexing (right to left)
print(text[-1])  # 'n'
print(text[-2])  # 'o'
print(text[-6])  # 'P'
```

### String Slicing

```python
url = "https://example.com/admin/login"

# Syntax: string[start:end:step]
print(url[0:5])     # 'https'
print(url[8:19])    # 'example.com'
print(url[20:])     # 'admin/login'
print(url[:5])      # 'https'
print(url[::2])     # 'hts/xml.o/diloii'
print(url[::-1])    # Reverse: 'nigol/nimda/moc.elpmaxe//:sptth'
```

**Security Example:**
```python
# Extract domain from URL
url = "https://malicious-site.com/phishing"
domain_start = url.find("://") + 3
domain_end = url.find("/", domain_start)
domain = url[domain_start:domain_end]
print(f"Domain: {domain}")  # malicious-site.com
```

---

## String Methods

### Case Manipulation

```python
text = "Hello World"

print(text.upper())       # 'HELLO WORLD'
print(text.lower())       # 'hello world'
print(text.capitalize())  # 'Hello world'
print(text.title())       # 'Hello World'
print(text.swapcase())    # 'hELLO wORLD'
```

**Security Example:**
```python
# Case-insensitive comparison
user_input = "ADmiN"
if user_input.lower() == "admin":
    print("Admin access detected")
```

### Searching & Finding

```python
log = "Failed login attempt from 192.0.2.50"

# Check if substring exists
if "Failed" in log:
    print("⚠️ Login failure detected")

# Find position
position = log.find("192.0.2.50")  # Returns 28
position = log.find("success")      # Returns -1 (not found)

# Count occurrences
text = "password password password"
count = text.count("password")  # 3

# Check start/end
url = "https://secure.example.com"
print(url.startswith("https"))  # True
print(url.endswith(".com"))     # True
```

### Replacing & Removing

```python
# Replace text
message = "User john logged in"
message = message.replace("john", "admin")
print(message)  # 'User admin logged in'

# Remove whitespace
username = "  admin  "
print(username.strip())   # 'admin'
print(username.lstrip())  # 'admin  '
print(username.rstrip())  # '  admin'

# Remove specific characters
phone = "+1-555-123-4567"
clean = phone.strip("+").replace("-", "")
print(clean)  # '15551234567'
```

### Splitting & Joining

```python
# Split string into list
log = "192.0.2.10,admin,login_success"
parts = log.split(",")
print(parts)  # ['192.0.2.10', 'admin', 'login_success']

ip, user, action = log.split(",")  # Unpack
print(f"IP: {ip}, User: {user}")

# Join list into string
words = ["secure", "password", "123"]
password = "-".join(words)
print(password)  # 'secure-password-123'

# Split by newlines
logs = """192.0.2.10 - Failed
192.0.2.11 - Success
192.0.2.12 - Failed"""

for line in logs.split("\n"):
    if "Failed" in line:
        print(f"⚠️ {line}")
```

---

## String Formatting

### F-Strings (Recommended - Python 3.6+)

```python
name = "Alice"
age = 25
ip = "192.0.2.10"

# Basic usage
message = f"User {name} is {age} years old"
print(message)

# Expressions
print(f"Next year: {age + 1}")

# Security logging
print(f"[ALERT] Failed login from {ip} by user '{name}'")

# Formatting numbers
port = 8080
print(f"Port: {port:05d}")  # 'Port: 08080' (5 digits, zero-padded)

score = 95.6789
print(f"Score: {score:.2f}")  # 'Score: 95.68' (2 decimal places)
```

### format() Method

```python
template = "User {} attempted login from {}"
message = template.format("admin", "192.0.2.50")

# With names
template = "User {user} from {ip}"
message = template.format(user="admin", ip="192.0.2.50")
```

### % Formatting (Old Style)

```python
name = "Alice"
age = 25

message = "User %s is %d years old" % (name, age)
print(message)
```

---

## Numbers

### Integers

```python
# Whole numbers
port = 8080
max_attempts = 5
status_code = 404

# Operations
total = 100 + 50
difference = 100 - 50
product = 10 * 5
quotient = 100 / 4    # 25.0 (float division)
floor_div = 100 // 4  # 25 (integer division)
remainder = 100 % 4   # 0 (modulo)
power = 2 ** 8        # 256
```

### Floats

```python
# Decimal numbers
score = 95.5
response_time = 0.245
percentage = 99.9

# Operations
average = (95.5 + 87.3 + 92.1) / 3
print(f"Average: {average:.2f}")
```

### Number Methods

```python
# Absolute value
distance = abs(-10)  # 10

# Rounding
score = 95.678
print(round(score))      # 96
print(round(score, 1))   # 95.7
print(round(score, 2))   # 95.68

# Min/Max
ports = [80, 443, 8080, 22, 21]
print(min(ports))  # 21
print(max(ports))  # 8080
print(sum(ports))  # 8646
```

---

## Type Conversion

### To String

```python
age = 25
port = 8080
score = 95.5

age_str = str(age)      # '25'
port_str = str(port)    # '8080'
score_str = str(score)  # '95.5'

# Concatenation requires strings
print("Port: " + str(port))
```

### To Integer

```python
age_str = "25"
age = int(age_str)  # 25

# From float
score = int(95.7)  # 95 (truncates decimal)

# Handle invalid input
user_input = "abc"
try:
    number = int(user_input)
except ValueError:
    print("⚠️ Invalid number!")
```

### To Float

```python
score_str = "95.5"
score = float(score_str)  # 95.5

# From integer
age = 25
age_float = float(age)  # 25.0
```

### Checking Types

```python
age = 25
name = "Alice"
score = 95.5

print(type(age))    # <class 'int'>
print(type(name))   # <class 'str'>
print(type(score))  # <class 'float'>

# isinstance() for checking
if isinstance(age, int):
    print("age is an integer")
```

---

## Security Examples

### 1. Log Parser

```python
log_entry = "2024-01-15 14:30:45 - Failed login from 192.0.2.50 - User: admin"

# Extract timestamp
timestamp = log_entry[:19]
print(f"Time: {timestamp}")

# Extract IP
ip_start = log_entry.find("from ") + 5
ip_end = log_entry.find(" - User")
ip = log_entry[ip_start:ip_end]
print(f"IP: {ip}")

# Extract username
username = log_entry.split("User: ")[1]
print(f"User: {username}")

# Check for failed attempt
if "Failed" in log_entry:
    print(f"⚠️ ALERT: Failed login detected from {ip}")
```

### 2. URL Parser

```python
url = "https://api.example.com:8080/v1/users?id=123&token=abc"

# Extract protocol
protocol = url.split("://")[0]  # 'https'

# Extract domain
domain_start = url.find("://") + 3
domain_end = url.find(":", domain_start)
if domain_end == -1:
    domain_end = url.find("/", domain_start)
domain = url[domain_start:domain_end]  # 'api.example.com'

# Extract port
if ":" in url[domain_start:]:
    port_start = url.find(":", domain_start) + 1
    port_end = url.find("/", port_start)
    port = url[port_start:port_end]  # '8080'
else:
    port = "443" if protocol == "https" else "80"

print(f"Protocol: {protocol}, Domain: {domain}, Port: {port}")
```

### 3. Password Validator

```python
password = "SecureP@ss123"

# Check length
if len(password) < 8:
    print("❌ Password too short (min 8 characters)")
else:
    print("✅ Length OK")

# Check for uppercase
if any(c.isupper() for c in password):
    print("✅ Contains uppercase")

# Check for lowercase
if any(c.islower() for c in password):
    print("✅ Contains lowercase")

# Check for numbers
if any(c.isdigit() for c in password):
    print("✅ Contains numbers")

# Check for special characters
special_chars = "!@#$%^&*()_+-=[]{}|;:',.<>?/"
if any(c in special_chars for c in password):
    print("✅ Contains special characters")
```

### 4. IP Address Validation (Basic)

```python
ip = "192.0.2.256"  # Invalid (256 > 255)

octets = ip.split(".")

if len(octets) != 4:
    print("❌ Invalid IP format")
else:
    valid = True
    for octet in octets:
        if not octet.isdigit():
            valid = False
            break
        num = int(octet)
        if num < 0 or num > 255:
            valid = False
            break
    
    if valid:
        print(f"✅ {ip} is valid")
    else:
        print(f"❌ {ip} is invalid")
```

### 5. User Input Sanitization

```python
# User input from web form
user_input = "  <script>alert('XSS')</script>  "

# Clean input
sanitized = user_input.strip()  # Remove whitespace
sanitized = sanitized.lower()   # Normalize case

# Remove dangerous characters
dangerous_chars = "<>\"'&"
for char in dangerous_chars:
    sanitized = sanitized.replace(char, "")

print(f"Original: {user_input}")
print(f"Sanitized: {sanitized}")
```

---

## Practice Exercises

1. **String Manipulation**: Extract the filename from a full file path
   ```python
   path = "/var/log/auth.log"
   # Expected output: "auth.log"
   ```

2. **Log Analysis**: Count how many times "Failed" appears in a log file
   ```python
   logs = "Failed\nSuccess\nFailed\nSuccess\nFailed\nFailed"
   # Expected output: 4
   ```

3. **Data Cleaning**: Remove all non-numeric characters from a phone number
   ```python
   phone = "+1 (555) 123-4567"
   # Expected output: "15551234567"
   ```

4. **Format Conversion**: Convert a list of IPs to a formatted report
   ```python
   ips = ["192.0.2.10", "192.0.2.11", "192.0.2.12"]
   # Expected: "IP 1: 192.0.2.10\nIP 2: 192.0.2.11\nIP 3: 192.0.2.12"
   ```

---

## Quick Reference

### String Methods Cheat Sheet

| Method | Purpose | Example |
|--------|---------|---------|
| `.upper()` | To uppercase | `"hello".upper()` → `"HELLO"` |
| `.lower()` | To lowercase | `"HELLO".lower()` → `"hello"` |
| `.strip()` | Remove whitespace | `" hi ".strip()` → `"hi"` |
| `.split()` | Split to list | `"a,b,c".split(",")` → `['a','b','c']` |
| `.replace()` | Replace text | `"hi".replace("i","o")` → `"ho"` |
| `.find()` | Find position | `"hello".find("l")` → `2` |
| `.count()` | Count occurrences | `"aaa".count("a")` → `3` |
| `.startswith()` | Check start | `"hi".startswith("h")` → `True` |
| `.endswith()` | Check end | `"hi".endswith("i")` → `True` |

---

## See Also

- [Operators](operators.md) - Work with data
- [Dictionaries](dictionaries.md) - Store key-value pairs
- [Functions](functions.md) - Organize your code
