# Python Operators

Master Python operators for calculations, comparisons, and logical operations - essential for security scripting and automation.

---

## 📚 Table of Contents

- [Assignment Operators](#assignment-operators)
- [Arithmetic Operators](#arithmetic-operators)
- [Comparison Operators](#comparison-operators)
- [Logical Operators](#logical-operators)
- [Bitwise Operators](#bitwise-operators)
- [Security Use Cases](#security-use-cases)

---

## Assignment Operators

Assignment operators are used to assign values to variables.

### Basic Assignment

```python
# Simple assignment
a = 0  # a equals 0

# Multiple assignment
x = y = z = 10  # All equal 10

# Multiple variables
name, age, city = "Alice", 25, "New York"
```

### Compound Assignment

```python
a = 10

a += 1   # a = a + 1  →  11
a -= 1   # a = a - 1  →  10  
a *= 2   # a = a * 2  →  20
a /= 5   # a = a / 5  →  4.0
a **= 3  # a = a ** 3 →  64.0 (power)
a //= 2  # a = a // 2 →  32.0 (floor division)
a %= 5   # a = a % 5  →  2.0 (modulo - remainder)
```

**Security Example:**
```python
# Track failed login attempts
failed_attempts = 0
failed_attempts += 1  # Increment on each failure

if failed_attempts >= 3:
    print("Account locked!")
```

---

## Arithmetic Operators

Perform mathematical operations.

| Operator | Name | Example | Result |
|----------|------|---------|--------|
| `+` | Addition | `5 + 3` | `8` |
| `-` | Subtraction | `5 - 3` | `2` |
| `*` | Multiplication | `5 * 3` | `15` |
| `/` | Division | `5 / 2` | `2.5` |
| `//` | Floor Division | `5 // 2` | `2` |
| `%` | Modulus | `5 % 2` | `1` |
| `**` | Exponentiation | `5 ** 2` | `25` |

### Examples

```python
# Basic math
total_ports = 65535
scanned = 1000
remaining = total_ports - scanned  #  64535

# Port range calculation
ports_per_thread = 65535 // 10  # 6553 ports per thread

# Check if port is in range
port = 8080
if port % 2 == 0:
    print("Even port number")
```

**Security Example:**
```python
# Calculate password strength score
password = "Str0ng!P@ss"
length_score = len(password) * 4
variety_score = 10  # has upper, lower, numbers, special

total_score = length_score + variety_score
print(f"Password strength: {total_score}")
```

---

## Comparison Operators

Compare values and return `True` or `False`.

| Operator | Description | Example | Result |
|----------|-------------|---------|--------|
| `==` | Equal to | `5 == 5` | `True` |
| `!=` | Not equal to | `5 != 3` | `True` |
| `>` | Greater than | `5 > 3` | `True` |
| `<` | Less than | `5 < 3` | `False` |
| `>=` | Greater or equal | `5 >= 5` | `True` |
| `<=` | Less or equal | `5 <= 3` | `False` |

### Examples

```python
# Port validation
port = 8080
if port >= 1 and port <= 65535:
    print("Valid port")

# Version check
current_version = "2.5.1"
required_version = "2.0.0"
if current_version >= required_version:
    print("Version OK")

# Status code check
status_code = 200
if status_code == 200:
    print("Success!")
elif status_code == 404:
    print("Not found")
```

**Security Example:**
```python
# Check response for SQL injection
response = "You have an error in your SQL syntax"

if "SQL" in response or "syntax" in response:
    print("⚠️ Possible SQL injection vulnerability!")
```

---

## Logical Operators

Combine conditional statements.

### AND Operator

Both conditions must be `True`:

```python
age = 25
has_license = True

if age >= 18 and has_license:
    print("Can drive")

# Security example
authenticated = True
has_permission = True

if authenticated and has_permission:
    print("Access granted")
```

### OR Operator

At least one condition must be `True`:

```python
is_admin = False
is_moderator = True

if is_admin or is_moderator:
    print("Has elevated privileges")

# Security example - check for common ports
port = 443

if port == 80 or port == 443 or port == 8080:
    print("Common HTTP/HTTPS port")
```

### NOT Operator

Reverse the boolean value:

```python
is_banned = False

if not is_banned:
    print("User allowed")

# Security example
ssl_enabled = False

if not ssl_enabled:
    print("⚠️ WARNING: SSL not enabled!")
```

### Combined Logical Operators

```python
# Complex access control
is_admin = False
is_owner = True
is_premium = True

if (is_admin or is_owner) and is_premium:
    print("Full access granted")

# Security check
has_firewall = True
has_antivirus = True
is_updated = False

if has_firewall and has_antivirus and is_updated:
    print("System secure")
else:
    print("⚠️ Security improvements needed")
```

---

## Bitwise Operators

Operate on binary representations of numbers.

| Operator | Name | Example | Binary | Result |
|----------|------|---------|--------|--------|
| `&` | AND | `5 & 3` | `101 & 011` | `1` (001) |
| `\|` | OR | `5 \| 3` | `101 \| 011` | `7` (111) |
| `^` | XOR | `5 ^ 3` | `101 ^ 011` | `6` (110) |
| `~` | NOT | `~5` | `~101` | `-6` |
| `<<` | Left Shift | `5 << 1` | `101 << 1` | `10` (1010) |
| `>>` | Right Shift | `5 >> 1` | `101 >> 1` | `2` (10) |

### Examples

```python
a = 5  # Binary: 101
b = 3  # Binary: 011

print(a & b)   # AND: 1 (001)
print(a | b)   # OR: 7 (111)
print(a ^ b)   # XOR: 6 (110)
print(~a)      # NOT: -6
print(a << 1)  # Left shift: 10 (1010)
print(a >> 1)  # Right shift: 2 (10)
```

**Security Use Cases:**

```python
# Subnet mask calculation
ip = 192
mask = 255

network = ip & mask  # AND operation for network address

# Permission flags (common in Linux file permissions)
READ = 4    # 100
WRITE = 2   # 010
EXECUTE = 1 # 001

# Check permissions
user_permission = 6  # READ + WRITE (4 + 2 = 6)

if user_permission & READ:
    print("Has read permission")
if user_permission & WRITE:
    print("Has write permission")
if user_permission & EXECUTE:
    print("Has execute permission")

# Set multiple permissions
admin_permission = READ | WRITE | EXECUTE  # 7 (111)
```

---

## Security Use Cases

### 1. Brute Force Attack Counter

```python
max_attempts = 5
attempts = 0

while attempts < max_attempts:
    password = input("Enter password: ")
    
    if password == "SecureP@ss123":
        print("✅ Access granted!")
        break
    else:
        attempts += 1
        remaining = max_attempts - attempts
        print(f"❌ Wrong password. {remaining} attempts remaining")

if attempts >= max_attempts:
    print("🔒 Account locked!")
```

### 2. Port Range Validation

```python
def is_valid_port(port):
    return port >= 1 and port <= 65535

# Test
ports = [80,443, 70000, -1, 8080]

for port in ports:
    if is_valid_port(port):
        print(f"✅ {port} is valid")
    else:
        print(f"❌ {port} is invalid")
```

### 3. HTTP Status Code Handler

```python
status_code = 403

if status_code == 200:
    print("✅ Success")
elif status_code >= 300 and status_code < 400:
    print("↪️ Redirect")
elif status_code >= 400 and status_code < 500:
    print("❌ Client Error")
elif status_code >= 500:
    print("⚠️ Server Error")
```

### 4. Permission Checker

```python
# Linux-style file permissions
user_perm = 6  # rw-
group_perm = 4  # r--
other_perm = 4  # r--

def check_permission(perm):
    permissions = []
    if perm & 4:  # Read
        permissions.append('r')
    if perm & 2:  # Write
        permissions.append('w')
    if perm & 1:  # Execute
        permissions.append('x')
    return ''.join(permissions) if permissions else '-'

print(f"File permissions: {check_permission(user_perm)}{check_permission(group_perm)}{check_permission(other_perm)}")
# Output: rw-r--r--
```

---

## Quick Reference

### Operator Precedence (High to Low)

1. `**` - Exponentiation
2. `~`, `+`, `-` - Bitwise NOT, unary plus/minus
3. `*`, `/`, `//`, `%` - Multiplication, division
4. `+`, `-` - Addition, subtraction
5. `<<`, `>>` - Bit shifts
6. `&` - Bitwise AND
7. `^` - Bitwise XOR
8. `|` - Bitwise OR
9. `==`, `!=`, `>`, `<`, `>=`, `<=` - Comparisons
10. `not` - Logical NOT
11. `and` - Logical AND
12. `or` - Logical OR

### Tips

- Use parentheses `()` for clarity: `(a and b) or c`
- Comparison chains work: `1 < x < 10`
- `is` vs `==`: Use `is` for identity, `==` for equality
- Avoid complex bitwise operations unless necessary

---

## Practice Exercises

1. Create a script that checks if a port is in the "well-known" range (0-1023)
2. Write a password strength checker using comparison operators
3. Build a simple access control system using logical operators
4. Calculate file permissions using bitwise operators

---

## See Also

- [Strings & Variables](strings-and-variables.md) - Data type basics
- [Dictionaries](dictionaries.md) - Data structures
- [Functions](functions.md) - Code organization
