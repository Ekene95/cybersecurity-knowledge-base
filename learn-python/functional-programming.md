# Map, Lambda, Zip & Filters

Master functional programming techniques for efficient data processing - essential for transforming security data and analyzing results.

---

## 📚 Table of Contents

- [What is Functional Programming?](#what-is-functional-programming)
- [Lambda Functions](#lambda-functions)
- [Map Function](#map-function)
- [Filter Function](#filter-function)
- [Zip Function](#zip-function)
- [Combining Techniques](#combining-techniques)
- [Security Use Cases](#security-use-cases)

---

## What is Functional Programming?

Functional programming is a style where you treat functions as "first-class citizens" - meaning functions can be:
- Passed as arguments to other functions
- Returned from functions
- Stored in variables

**Benefits for Security:**
- ✅ Clean, concise code
- ✅ Easy to test
- ✅ Fewer bugs
- ✅ Efficient data processing

---

## Lambda Functions

Lambda functions are small, anonymous functions defined in a single line.

### Basic Lambda Syntax

```python
# Regular function
def add(x, y):
    return x + y

# Lambda equivalent
add_lambda = lambda x, y: x + y

print(add(5, 3))         # 8
print(add_lambda(5, 3))  # 8
```

**Syntax:** `lambda arguments: expression`

### Common Lambda Uses

```python
# Single argument
square = lambda x: x ** 2
print(square(5))  # 25

# Multiple arguments
multiply = lambda x, y: x * y
print(multiply(4, 5))  # 20

# No arguments
greet = lambda: "Hello, World!"
print(greet())  # "Hello, World!"

# Conditional logic
is_adult = lambda age: "Adult" if age >= 18 else "Minor"
print(is_adult(25))  # "Adult"
print(is_adult(15))  # "Minor"
```

### Lambda for Sorting

```python
# Sort by second element
pairs = [(1, 5), (3, 2), (2, 8), (4, 1)]
sorted_pairs = sorted(pairs, key=lambda x: x[1])
print(sorted_pairs)  # [(4, 1), (3, 2), (1, 5), (2, 8)]

# Sort vulnerabilities by severity score
vulns = [
    {"name": "XSS", "score": 7.5},
    {"name": "SQLi", "score": 9.8},
    {"name": "CSRF", "score": 6.1}
]
sorted_vulns = sorted(vulns, key=lambda v: v["score"], reverse=True)

for vuln in sorted_vulns:
    print(f"{vuln['score']}: {vuln['name']}")
```

---

## Map Function

`map()` applies a function to every item in an iterable.

**Syntax:** `map(function, iterable)`

### Basic Map Usage

```python
# Double all numbers
numbers = [1, 2, 3, 4, 5]
doubled = list(map(lambda x: x * 2, numbers))
print(doubled)  # [2, 4, 6, 8, 10]

# Convert to uppercase
names = ["alice", "bob", "charlie"]
upper_names = list(map(lambda name: name.upper(), names))
print(upper_names)  # ['ALICE', 'BOB', 'CHARLIE']

# String length
words = ["hello", "world", "python"]
lengths = list(map(len, words))  # Using built-in function
print(lengths)  # [5, 5, 6]
```

### Map with Multiple Iterables

```python
# Add corresponding elements
list1 = [1, 2, 3]
list2 = [10, 20, 30]
sums = list(map(lambda x, y: x + y, list1, list2))
print(sums)  # [11, 22, 33]

# Combine first and last names
first_names = ["Alice", "Bob", "Charlie"]
last_names = ["Smith", "Jones", "Brown"]
full_names = list(map(lambda f, l: f + " " + l, first_names, last_names))
print(full_names)  # ['Alice Smith', 'Bob Jones', 'Charlie Brown']
```

### Map vs List Comprehension

```python
numbers = [1, 2, 3, 4, 5]

# Using map
squared_map = list(map(lambda x: x ** 2, numbers))

# Using list comprehension (often more readable)
squared_comp = [x ** 2 for x in numbers]

print(squared_map)   # [1, 4, 9, 16, 25]
print(squared_comp)  # [1, 4, 9, 16, 25]
```

**When to use map:**
- When applying existing function
- When working with multiple iterables
- When you want lazy evaluation (map returns iterator)

**When to use list comprehension:**
- When you need filtering (`if` clause)
- When you want immediate list
- When readability matters

---

## Filter Function

`filter()` selects items from an iterable where function returns `True`.

**Syntax:** `filter(function, iterable)`

### Basic Filter Usage

```python
# Filter even numbers
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
even = list(filter(lambda x: x % 2 == 0, numbers))
print(even)  # [2, 4, 6, 8, 10]

# Filter valid ports
ports = [80, 443, -1, 70000, 8080, 22, 65536]
valid = list(filter(lambda p: 1 <= p <= 65535, ports))
print(valid)  # [80, 443, 8080, 22]

# Filter strings containing 'a'
words = ["apple", "banana", "cherry", "date"]
has_a = list(filter(lambda word: 'a' in word, words))
print(has_a)  # ['apple', 'banana', 'date']
```

### Filter with None

```python
# Remove falsy values (0, '', None, False, etc.)
mixed = [0, 1, "", "hello", None, True, False, [], [1, 2]]
truthy = list(filter(None, mixed))
print(truthy)  # [1, 'hello', True, [1, 2]]
```

### Filter vs List Comprehension

```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Using filter
even_filter = list(filter(lambda x: x % 2 == 0, numbers))

# Using list comprehension (often more readable)
even_comp = [x for x in numbers if x % 2 == 0]

print(even_filter)  # [2, 4, 6, 8, 10]
print(even_comp)    # [2, 4, 6, 8, 10]
```

---

## Zip Function

`zip()` combines multiple iterables element-by-element into tuples.

**Syntax:** `zip(iterable1, iterable2, ...)`

### Basic Zip Usage

```python
# Combine two lists
names = ["Alice", "Bob", "Charlie"]
ages = [25, 30, 35]

combined = list(zip(names, ages))
print(combined)  # [('Alice', 25), ('Bob', 30), ('Charlie', 35)]

# Unpack in loop
for name, age in zip(names, ages):
    print(f"{name} is {age} years old")
```

### Zip Multiple Lists

```python
ips = ["192.0.2.10", "192.0.2.11", "192.0.2.12"]
ports = [80, 443, 22]
status = ["open", "closed", "open"]

for ip, port, stat in zip(ips, ports, status):
    print(f"{ip}:{port} - {stat}")
```

**Output:**
```
192.0.2.10:80 - open
192.0.2.11:443 - closed
192.0.2.12:22 - open
```

### Zip with Different Lengths

```python
# zip stops at shortest iterable
list1 = [1, 2, 3, 4, 5]
list2 = ['a', 'b', 'c']

zipped = list(zip(list1, list2))
print(zipped)  # [(1, 'a'), (2, 'b'), (3, 'c')]
```

### Unzipping

```python
# Create pairs
pairs = [(1, 'a'), (2, 'b'), (3, 'c')]

# Unzip using * unpacking
numbers, letters = zip(*pairs)
print(numbers)  # (1, 2, 3)
print(letters)  # ('a', 'b', 'c')
```

### Creating Dictionaries with Zip

```python
keys = ["name", "age", "role"]
values = ["Alice", 25, "admin"]

# Create dictionary
user = dict(zip(keys, values))
print(user)  # {'name': 'Alice', 'age': 25, 'role': 'admin'}
```

---

## Combining Techniques

### Map + Filter

```python
# Get squares of even numbers
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Method 1: Filter then map
even = filter(lambda x: x % 2 == 0, numbers)
squared = list(map(lambda x: x ** 2, even))
print(squared)  # [4, 16, 36, 64, 100]

# Method 2: List comprehension (cleaner)
squared_comp = [x ** 2 for x in numbers if x % 2 == 0]
print(squared_comp)  # [4, 16, 36, 64, 100]
```

### Zip + Map

```python
# Calculate distance between coordinates
x_coords = [1, 3, 5]
y_coords = [2, 4, 6]

distances = list(map(lambda x, y: ((x**2 + y**2) ** 0.5), x_coords, y_coords))
print([round(d, 2) for d in distances])  # [2.24, 5.0, 7.81]
```

### All Three Together

```python
# Complex data processing pipeline
names = ["Alice", "bob", "CHARLIE", "dave"]
scores = [85, 92, 78, 95]
passing_score = 80

# 1. Normalize names (map)
normalized = map(lambda name: name.capitalize(), names)

# 2. Combine with scores (zip)
student_data = zip(normalized, scores)

# 3. Filter passing students (filter)
passing = filter(lambda student: student[1] >= passing_score, student_data)

# Convert to list and print
results = list(passing)
print(results)  # [('Alice', 85), ('Bob', 92), ('Dave', 95)]
```

---

## Security Use Cases

### 1. Port Status Checker

```python
# Simulated port scan results
scanned_ports = [21, 22, 80, 443, 3306, 8080]
port_status = [False, True, True, True, False, True]

# Combine ports with status
scan_results = dict(zip(scanned_ports, port_status))

# Filter open ports
open_ports = list(filter(lambda p: scan_results[p], scanned_ports))
print(f"Open ports: {open_ports}")

# Map to service names
service_map = {
    21: "FTP", 22: "SSH", 80: "HTTP",
    443: "HTTPS", 3306: "MySQL", 8080: "HTTP-Alt"
}

open_services = list(map(lambda p: f"{p} ({service_map.get(p, 'Unknown')})", open_ports))
print("Open services:")
for service in open_services:
    print(f"  - {service}")
```

### 2. Vulnerability Severity Calculator

```python
# CVE scores from different sources
nvd_scores = [7.5, 9.8, 6.1, 10.0, 5.3]
vendor_scores = [7.0, 9.5, 6.5, 10.0, 5.0]

# Calculate average severity
avg_scores = list(map(lambda n, v: (n + v) / 2, nvd_scores, vendor_scores))

# Filter critical vulnerabilities (score >= 9.0)
critical = list(filter(lambda score: score >= 9.0, avg_scores))

print(f"Average scores: {avg_scores}")
print(f"Critical vulnerabilities: {len(critical)}")
```

### 3. IP Address Processor

```python
# List of IPs with different formats
raw_ips = [
    "  192.0.2.10  ",
    "192.0.2.11",
    "  192.0.2.12",
    "192.0.2.256",  # Invalid
    "192.0.2.13  "
]

# 1. Clean whitespace (map)
cleaned = map(lambda ip: ip.strip(), raw_ips)

# 2. Filter valid IPs (filter)
def is_valid_ip(ip):
    octets = ip.split(".")
    if len(octets) != 4:
        return False
    return all(o.isdigit() and 0 <= int(o) <= 255 for o in octets)

valid_ips = list(filter(is_valid_ip, cleaned))

#  3. Add subnet mask (map)
with_subnet = list(map(lambda ip: f"{ip}/24", valid_ips))

print("Valid IPs with subnet:")
for ip in with_subnet:
    print(f"  - {ip}")
```

### 4. Password Strength Batch Checker

```python
passwords = [
    "pass",
    "Password1",
    "SecureP@ss2024",
    "admin",
    "MyStr0ng!Pass"
]

def calculate_strength(password):
    """Return password strength score"""
    score = 0
    if len(password) >= 8: score += 25
    if any(c.isupper() for c in password): score += 25
    if any(c.islower() for c in password): score += 25
    if any(c.isdigit() for c in password): score += 15
    if any(c in "!@#$%^&*()" for c in password): score += 10
    return min(score, 100)

# Calculate scores for all passwords
scores = list(map(calculate_strength, passwords))

# Combine passwords with scores
pwd_scores = list(zip(passwords, scores))

# Filter weak passwords (score < 50)
weak_passwords = list(filter(lambda ps: ps[1] < 50, pwd_scores))

print("⚠️ Weak passwords detected:")
for pwd, score in weak_passwords:
    print(f"  '{pwd}': {score}/100")
```

### 5. Log Entry Parser

```python
log_entries = [
    "2024-01-15 10:30:00 - ERROR - 192.0.2.50 - Failed login",
    "2024-01-15 10:31:00 - INFO  - 192.0.2.51 - Success",
    "2024-01-15 10:32:00 - ERROR - 192.0.2.50 - Failed login",
    "2024-01-15 10:33:00 - WARN  - 192.0.2.52 - Suspicious",
    "2024-01-15 10:34:00 - ERROR - 192.0.2.50 - Failed login"
]

# Parse log entries into dictionaries
parse_log = lambda log: {
    "timestamp": log.split(" - ")[0],
    "level": log.split(" - ")[1].strip(),
    "ip": log.split(" - ")[2],
    "message": log.split(" - ")[3]
}

parsed_logs = list(map(parse_log, log_entries))

# Filter only errors
errors = list(filter(lambda log: log["level"] == "ERROR", parsed_logs))

# Extract unique IPs with errors
error_ips = set(map(lambda log: log["ip"], errors))

print(f"Found {len(errors)} errors from {len(error_ips)} unique IPs")
print("IPs with errors:")
for ip in error_ips:
    count = sum(1 for log in errors if log["ip"] == ip)
    print(f"  {ip}: {count} errors")
```

---

## Performance Considerations

### Lazy Evaluation

```python
# map() and filter() return iterators (lazy)
numbers = range(1000000)  # Million numbers

# This doesn't compute anything yet!
doubled = map(lambda x: x * 2, numbers)

# Only computes when you iterate
for i, num in enumerate(doubled):
    print(num)
    if i >= 4:  # Only processes 5 numbers
        break
```

### Memory Efficiency

```python
import sys

# List comprehension - creates full list in memory
numbers = [x ** 2 for x in range(1000000)]
print(f"List size: {sys.getsizeof(numbers)} bytes")

# map() - iterator, uses minimal memory
numbers_map = map(lambda x: x ** 2, range(1000000))
print(f"Map size: {sys.getsizeof(numbers_map)} bytes")
```

---

## Quick Reference

| Function | Purpose | Example |
|----------|---------|---------|
| `lambda` | Anonymous function | `lambda x: x * 2` |
| `map()` | Apply function to all items | `map(func, list)` |
| `filter()` | Select items by condition | `filter(func, list)` |
| `zip()` | Combine multiple lists | `zip(list1, list2)` |

---

## Practice Exercises

1. Use `map()` to convert list of temperatures from Celsius to Fahrenheit
2. Use `filter()` to extract passwords shorter than 8 characters
3. Use `zip()` to create dictionary from two lists
4. Combine all three to process user data: clean, validate, and format

---

## See Also

- [Functions](functions.md) - Create functions
- [Iteration](iteration.md) - Loops and iteration
- [List Comprehensions](iteration.md#list-comprehensions) - Alternative syntax
