# Iteration & Iterators

Master loops and iteration - the foundation for processing data, analyzing logs, and automating security tasks.

---

## 📚 Table of Contents

- [What is Iteration?](#what-is-iteration)
- [For Loops](#for-loops)
- [While Loops](#while-loops)
- [Iterables and Iterators](#iterables-and-iterators)
- [List Comprehensions](#list-comprehensions)
- [Advanced Iteration](#advanced-iteration)
- [Security Examples](#security-examples)

---

## What is Iteration?

**Iteration** is the process of repeating a set of instructions until a condition is met or all items in a collection have been processed.

```python
# Instead of writing this:
print("Scanning port 80...")
print("Scanning port 443...")
print("Scanning port 8080...")

# Use iteration:
ports = [80, 443, 8080]
for port in ports:
    print(f"Scanning port {port}...")
```

**Key Terms:**
- **Iteration**: One complete execution of a loop
- **Iterable**: An object you can loop over (list, string, dictionary, etc.)
- **Iterator**: An object that produces values one at a time

---

## For Loops

Use `for` loops when you know how many times to iterate or have a collection to process.

### Basic For Loop

```python
# Iterate over list
ports = [21, 22, 80, 443]
for port in ports:
    print(f"Port {port}")

# Iterate over string
for char in "HELLO":
    print(char)

# Iterate over range
for i in range(5):
    print(i)  # 0, 1, 2, 3, 4
```

### Range() Function

```python
# range(stop)
for i in range(5):
    print(i)  # 0 to 4

# range(start, stop)
for i in range(1, 6):
    print(i)  # 1 to 5

# range(start, stop, step)
for i in range(0, 10, 2):
    print(i)  # 0, 2, 4, 6, 8

# Reverse range
for i in range(10, 0, -1):
    print(i)  # 10 to 1
```

### Enumerate() - Get Index and Value

```python
vulnerabilities = ["XSS", "SQLi", "CSRF", "RCE"]

# Without enumerate (manual indexing)
for i in range(len(vulnerabilities)):
    print(f"{i}: {vulnerabilities[i]}")

# With enumerate (cleaner)
for index, vuln in enumerate(vulnerabilities):
    print(f"{index}: {vuln}")

# Start index from 1
for index, vuln in enumerate(vulnerabilities, start=1):
    print(f"{index}. {vuln}")
```

### Zip() - Iterate Multiple Lists

```python
ips = ["192.0.2.10", "192.0.2.11", "192.0.2.12"]
statuses = ["online", "offline", "online"]
ports = [80, 443, 22]

# Zip combines lists element by element
for ip, status, port in zip(ips, statuses, ports):
    print(f"{ip}:{port} is {status}")
```

**Output:**
```
192.0.2.10:80 is online
192.0.2.11:443 is offline
192.0.2.12:22 is online
```

### Dictionary Iteration

```python
user = {
    "username": "alice",
    "role": "admin",
    "age": 25
}

# Iterate over keys (default)
for key in user:
    print(key)

# Iterate over values
for value in user.values():
    print(value)

# Iterate over key-value pairs
for key, value in user.items():
    print(f"{key}: {value}")
```

---

## While Loops

Use `while` loops when you don't know how many iterations you'll need.

### Basic While Loop

```python
count = 0
while count < 5:
    print(f"Count: {count}")
    count += 1
```

### User Input Loop

```python
max_attempts = 3
attempts = 0

while attempts < max_attempts:
    password = input("Enter password: ")
    
    if password == "SecurePass123":
        print("✅ Access granted!")
        break
    else:
        attempts += 1
        remaining = max_attempts - attempts
        print(f"❌ Wrong password. {remaining} attempts remaining")

if attempts >= max_attempts:
    print("🔒 Account locked!")
```

### Infinite Loop (Use with Care!)

```python
# Infinite loop - must use break to exit
while True:
    command = input("Enter command (or 'quit'): ")
    
    if command == "quit":
        break
    
    print(f"Executing: {command}")
```

---

## Iterables and Iterators

Understanding how Python iteration works under the hood.

### What is an Iterable?

An **iterable** is any Python object that can be looped over. It has an `__iter__()` method that returns an iterator.

```python
# These are all iterables
a = "hello"          # String
b = [1, 2, 3]        # List
c = (1, 2, 3)        # Tuple
d = {1, 2, 3}        # Set
e = {"a": 1}         # Dictionary

# Check if something is iterable
print(dir(a))  # You'll see __iter__ in the list
```

### What is an Iterator?

An **iterator** is an object that produces values one at a time using the `__next__()` method.

```python
# Create an iterator from a string
text = "hello"
text_iterator = iter(text)

# Get each character one at a time
print(next(text_iterator))  # 'h'
print(next(text_iterator))  # 'e'
print(next(text_iterator))  # 'l'
print(next(text_iterator))  # 'l'
print(next(text_iterator))  # 'o'
# print(next(text_iterator))  # StopIteration error!

# Create iterator from list
numbers = [1, 2, 3]
numbers_iter = iter(numbers)

for num in numbers_iter:
    print(num)  # 1, 2, 3
```

### How For Loops Work

```python
# When you write this:
for char in "hello":
    print(char)

# Python actually does this:
text = "hello"
text_iterator = iter(text)  # Get iterator
while True:
    try:
        char = next(text_iterator)  # Get next value
        print(char)
    except StopIteration:  # When no more values
        break  # Exit loop
```

---

## List Comprehensions

Create new lists in a single line - fast and Pythonic!

### Basic List Comprehension

```python
# Traditional way
squares = []
for x in range(10):
    squares.append(x ** 2)
print(squares)

# List comprehension (better!)
squares = [x ** 2 for x in range(10)]
print(squares)  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

### With Condition

```python
# Only even numbers
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
even = [x for x in numbers if x % 2 == 0]
print(even)  # [2, 4, 6, 8, 10]

# Only valid ports
ports = [80, 443, -1, 70000, 8080, 22]
valid_ports = [p for p in ports if 1 <= p <= 65535]
print(valid_ports)  # [80, 443, 8080, 22]
```

### String Operations

```python
# Convert to uppercase
names = ["alice", "bob", "charlie"]
upper_names = [name.upper() for name in names]
print(upper_names)  # ['ALICE', 'BOB', 'CHARLIE']

# Extract domains from emails
emails = ["alice@example.com", "bob@test.org", "charlie@demo.net"]
domains = [email.split("@")[1] for email in emails]
print(domains)  # ['example.com', 'test.org', 'demo.net']
```

### Nested List Comprehension

```python
# Create multiplication table
table = [[i * j for j in range(1, 6)] for i in range(1, 6)]
for row in table:
    print(row)
```

---

## Advanced Iteration

### Break Statement

Exit a loop prematurely:

```python
# Stop when critical vulnerability found
vulnerabilities = ["Low", "Medium", "Low", "Critical", "High"]

for vuln in vulnerabilities:
    print(f"Found: {vuln}")
    if vuln == "Critical":
        print("⚠️ CRITICAL VULNERABILITY - Stopping scan!")
        break
```

### Continue Statement

Skip current iteration and move to next:

```python
# Skip non-critical vulnerabilities
vulnerabilities = ["Low", "Medium", "Critical", "High", "Critical"]

for vuln in vulnerabilities:
    if vuln in ["Low", "Medium"]:
        continue  # Skip to next iteration
    print(f"⚠️ {vuln} vulnerability found!")
```

### Else Clause in Loops

Execute code when loop completes normally (no break):

```python
max_attempts = 3

for attempt in range(max_attempts):
    password = input("Enter password: ")
    if password == "SecurePass123":
        print("✅ Access granted!")
        break
else:
    # This runs if loop wasn't broken
    print("🔒 Maximum attempts reached. Account locked!")
```

### Nested Loops

```python
# Scan multiple IPs on multiple ports
ips = ["192.0.2.10", "192.0.2.11"]
ports = [80, 443, 8080]

for ip in ips:
    print(f"\nScanning {ip}...")
    for port in ports:
        print(f"  Port {port}: open")
```

---

## Security Examples

### 1. Port Scanner (Simulation)

```python
import random

def scan_ports(ip, ports):
    """Simulate port scanning"""
    open_ports = []
    
    for port in ports:
        # Simulate random port status
        is_open = random.choice([True, False])
        
        if is_open:
            open_ports.append(port)
            print(f"  ✅ Port {port}: OPEN")
        else:
            print(f"  ❌ Port {port}: CLOSED")
    
    return open_ports

target = "192.0.2.10"
common_ports = [21, 22, 80, 443, 3306, 8080]

print(f"Scanning {target}...")
results = scan_ports(target, common_ports)
print(f"\n{len(results)} ports open: {results}")
```

### 2. Log File Analyzer

```python
logs = [
    "2024-01-15 10:30:00 - ERROR - 192.0.2.50 - Failed login",
    "2024-01-15 10:31:15 - INFO  - 192.0.2.51 - Success",
    "2024-01-15 10:32:30 - ERROR - 192.0.2.50 - Failed login",
    "2024-01-15 10:33:45 - ERROR - 192.0.2.52 - Failed login",
    "2024-01-15 10:34:00 - ERROR - 192.0.2.50 - Failed login"
]

# Count failed attempts per IP
failed_ips = {}

for log in logs:
    if "Failed login" in log:
        parts = log.split(" - ")
        ip = parts[2]
        
        if ip in failed_ips:
            failed_ips[ip] += 1
        else:
            failed_ips[ip] = 1

# Report IPs with multiple failures
print("⚠️ IPs with failed login attempts:")
for ip, count in failed_ips.items():
    if count >= 3:
        print(f"  🔒 {ip}: {count} attempts - BLOCKED!")
    else:
        print(f"  ⚠️ {ip}: {count} attempts")
```

### 3. Subdomain Generator

```python
def generate_subdomains(domain, prefixes):
    """Generate list of possible subdomains"""
    subdomains = []
    
    for prefix in prefixes:
        subdomain = f"{prefix}.{domain}"
        subdomains.append(subdomain)
    
    return subdomains

# Common subdomain prefixes
prefixes = ["www", "mail", "ftp", "admin", "api", "dev", "test", "staging"]
domain = "example.com"

subdomains = generate_subdomains(domain, prefixes)

print(f"Testing {len(subdomains)} subdomains:")
for subdomain in subdomains:
    print(f"  - {subdomain}")
```

### 4. Password List Generator

```python
def generate_password_variations(base_password, years, symbols):
    """Generate common password variations"""
    variations = [base_password]
    
    # Add year variations
    for year in years:
        variations.append(f"{base_password}{year}")
        variations.append(f"{year}{base_password}")
    
    # Add symbol variations
    for symbol in symbols:
        variations.append(f"{base_password}{symbol}")
        variations.append(f"{symbol}{base_password}")
    
    # Combine years and symbols
    for year in years:
        for symbol in symbols:
            variations.append(f"{base_password}{year}{symbol}")
    
    return variations

base = "admin"
years = [2023, 2024]
symbols = ["!", "@", "#"]

passwords = generate_password_variations(base, years, symbols)

print(f"Generated {len(passwords)} password variations:")
for i, pwd in enumerate(passwords[:10], 1):
    print(f"{i:2}. {pwd}")
print(f"... and {len(passwords) - 10} more")
```

### 5. IP Range Scanner

```python
def generate_ip_range(base, start, end):
    """Generate IP addresses in a range"""
    ips = []
    for i in range(start, end + 1):
        ips.append(f"{base}.{i}")
    return ips

# Scan 192.0.2.1 through 192.0.2.254
ip_base = "192.0.2"
targets = generate_ip_range(ip_base, 1, 254)

print(f"Scanning {len(targets)} IP addresses...")
for ip in targets[:5]:  # Show first 5
    print(f"  Scanning {ip}...")
print(f"  ... and {len(targets) - 5} more")
```

---

## Best Practices

1. **Use for when possible**: More readable than while
2. **Avoid modifying lists during iteration**: Create new list instead
3. **Use list comprehensions**: Faster and more Pythonic
4. **Break early**: Exit loops as soon as possible
5. **Use enumerate()**: Better than manual indexing

```python
# ❌ Bad - modifying during iteration
numbers = [1, 2, 3, 4, 5]
for num in numbers:
    if num % 2 == 0:
        numbers.remove(num)  # DON'T DO THIS!

# ✅ Good - create new list
numbers = [1, 2, 3, 4, 5]
odd_numbers = [num for num in numbers if num % 2 != 0]
```

---

## Practice Exercises

1. **FizzBuzz**: Print numbers 1-100, but "Fizz" for multiples of 3, "Buzz" for 5, "FizzBuzz" for both
2. **Prime Numbers**: Find all prime numbers between 1 and 100
3. **Character Counter**: Count character frequency in a string
4. **Palindrome Checker**: Check if a word reads the same backwards
5. **Fibonacci**: Generate first 20 Fibonacci numbers

---

## See Also

- [Functions](functions.md) - Create reusable code
- [Map, Lambda, Zip & Filters](functional-programming.md) - Advanced iteration
- [Dictionaries](dictionaries.md) - Iterate over data structures
