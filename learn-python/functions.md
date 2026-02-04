# Python Functions

Learn to create reusable, modular code - essential for building security tools and automating tasks.

---

## 📚 Table of Contents

- [What is a Function?](#what-is-a-function)
- [Creating Functions](#creating-functions)
- [Parameters and Arguments](#parameters-and-arguments)
- [Return Values](#return-values)
- [Args and Kwargs](#args-and-kwargs)
- [Scope and Variables](#scope-and-variables)
- [Lambda Functions](#lambda-functions)
- [Security Examples](#security-examples)

---

## What is a Function?

A function is a reusable block of code that performs a specific task. Instead of writing the same code multiple times, you write it once in a function and call it whenever needed.

**Benefits:**
- ✅ **Reusability** - Write once, use many times
- ✅ **Organization** - Break complex tasks into smaller pieces
- ✅ **Maintainability** - Fix bugs in one place
- ✅ **Testing** - Test individual functions easily

```python
# Without function (repetitive)
print("Checking port 80...")
if port_80_open:
    print("Port 80 is open")

print("Checking port 443...")
if port_443_open:
    print("Port 443 is open")

# With function (reusable)
def check_port(port, is_open):
    print(f"Checking port {port}...")
    if is_open:
        print(f"Port {port} is open")

check_port(80, True)
check_port(443, True)
```

---

## Creating Functions

### Basic Function

```python
def greet():
    print("Hello, World!")

# Call the function
greet()  # Output: Hello, World!
```

### Function with Parameters

```python
def greet_user(name):
    print(f"Hello, {name}!")

greet_user("Alice")  # Hello, Alice!
greet_user("Bob")    # Hello, Bob!
```

### Function with Multiple Parameters

```python
def scan_port(ip, port):
    print(f"Scanning {ip}:{port}...")

scan_port("192.0.2.10", 80)
scan_port("192.0.2.11", 443)
```

### Function with Default Parameters

```python
def scan_port(ip, port=80):
    print(f"Scanning {ip}:{port}...")

scan_port("192.0.2.10")          # Uses default port 80
scan_port("192.0.2.11", 443)     # Overrides default
```

---

## Parameters and Arguments

### Positional Arguments

Arguments are matched to parameters by position:

```python
def create_user(username, role, age):
    print(f"User: {username}, Role: {role}, Age: {age}")

create_user("alice", "admin", 25)  # Order matters!
```

### Keyword Arguments

Arguments are matched to parameters by name:

```python
def create_user(username, role, age):
    print(f"User: {username}, Role: {role}, Age: {age}")

# Order doesn't matter with keyword arguments
create_user(age=25, username="alice", role="admin")
create_user(role="admin", username="bob", age=30)
```

### Mixing Positional and Keyword

```python
def create_user(username, role, age):
    print(f"User: {username}, Role: {role}, Age: {age}")

# Positional first, then keyword
create_user("alice", role="admin", age=25)  # ✅ Valid
# create_user(username="alice", "admin", 25)  # ❌ Error!
```

### Default Parameters

```python
def scan_host(ip, port=80, timeout=30, verbose=False):
    msg = f"Scanning {ip}:{port} (timeout: {timeout}s)"
    if verbose:
        print(msg)
    return msg

# Use all defaults
scan_host("192.0.2.10")

# Override some defaults
scan_host("192.0.2.10", port=443)
scan_host("192.0.2.10", timeout=60, verbose=True)
```

---

## Return Values

Functions can return data back to the caller.

### Single Return Value

```python
def add(a, b):
    return a + b

result = add(5, 3)
print(result)  # 8

# Use directly
total = add(10, 20) + add(5, 5)
print(total)  # 35
```

### Multiple Return Values

```python
def get_user_info():
    username = "alice"
    role = "admin"
    age = 25
    return username, role, age  # Returns tuple

# Unpack returned values
user, user_role, user_age = get_user_info()
print(f"{user} is a {user_role}, age {user_age}")
```

### Early Return

```python
def is_valid_port(port):
    if port < 1 or port > 65535:
        return False
    return True

print(is_valid_port(80))     # True
print(is_valid_port(70000))  # False
```

### Return None

```python
def log_message(message):
    print(f"[LOG] {message}")
    # No return statement = returns None

result = log_message("System started")
print(result)  # None
```

---

## Args and Kwargs

### *args - Variable Positional Arguments

The `*args` parameter allows a function to accept any number of positional arguments.

```python
def scan_ports(ip, *ports):
    """
    Scan multiple ports on a single IP
    ip: target IP address
    *ports: variable number of ports to scan
    """
    print(f"Scanning {ip}...")
    for port in ports:
        print(f"  Checking port {port}")

# Can pass any number of ports
scan_ports("192.0.2.10", 80)
scan_ports("192.0.2.10", 80, 443, 8080)
scan_ports("192.0.2.10", 21, 22, 23, 25, 80, 443)
```

**How it works:**
- `*ports` collects all extra positional arguments into a tuple
- Inside the function, `ports` is a tuple you can iterate over

```python
def sum_all(*numbers):
    total = 0
    for num in numbers:
        total += num
    return total

print(sum_all(1, 2, 3))        # 6
print(sum_all(10, 20, 30, 40)) # 100
```

### **kwargs - Variable Keyword Arguments

The `**kwargs` parameter allows a function to accept any number of keyword arguments.

```python
def create_scan(**options):
    """
    Create a scan with flexible options
    **options: variable number of named parameters
    """
    print("Scan configuration:")
    for key, value in options.items():
        print(f"  {key}: {value}")

# Can pass any number of named arguments
create_scan(target="192.0.2.10", port=80, timeout=30)
create_scan(target="192.0.2.11", ports=[80, 443], threads=10, aggressive=True)
```

**How it works:**
- `**options` collects all extra keyword arguments into a dictionary
- Inside the function, `options` is a dictionary

```python
def build_user(**user_data):
    username = user_data.get("username", "unknown")
    role = user_data.get("role", "user")
    age = user_data.get("age", 0)
    
    return f"User: {username}, Role: {role}, Age: {age}"

print(build_user(username="alice", role="admin", age=25))
print(build_user(username="bob"))  # Uses defaults for role and age
```

### Combining Everything

```python
def advanced_scan(ip, *ports, **options):
    """
    ip: required positional argument
    *ports: variable number of ports
    **options: variable configuration options
    """
    timeout = options.get("timeout", 30)
    threads = options.get("threads", 1)
    verbose = options.get("verbose", False)
    
    print(f"Scanning {ip} on ports: {ports}")
    print(f"Timeout: {timeout}s, Threads: {threads}, Verbose: {verbose}")

# Usage
advanced_scan("192.0.2.10", 80, 443, timeout=60, threads=5)
advanced_scan("192.0.2.11", 22, 3306, verbose=True)
```

### Unpacking with * and **

```python
# Unpacking list with *
ports = [80, 443, 8080]
scan_ports("192.0.2.10", *ports)  # Same as scan_ports("192.0.2.10", 80, 443, 8080)

# Unpacking dictionary with **
config = {"timeout": 60, "threads": 10, "verbose": True}
advanced_scan("192.0.2.10", 80, 443, **config)
```

---

## Scope and Variables

### Local vs Global Variables

```python
# Global variable
max_attempts = 5

def check_login(attempts):
    # Local variable
    message = f"Attempts: {attempts}/{max_attempts}"
    return message

print(check_login(3))  # Attempts: 3/5
# print(message)  # ❌ Error! message is local to function
```

### Modifying Global Variables

```python
failed_attempts = 0

def record_failed_login():
    global failed_attempts  # Declare we're using global variable
    failed_attempts += 1
    print(f"Failed attempts: {failed_attempts}")

record_failed_login()  # Failed attempts: 1
record_failed_login()  # Failed attempts: 2
```

---

## Lambda Functions

Lambda functions are small, anonymous functions defined in one line.

### Basic Lambda

```python
# Regular function
def add(a, b):
    return a + b

# Lambda equivalent
add_lambda = lambda a, b: a + b

print(add(5, 3))         # 8
print(add_lambda(5, 3))  # 8
```

### Lambda with map()

```python
# Double all numbers
numbers = [1, 2, 3, 4, 5]
doubled = list(map(lambda x: x * 2, numbers))
print(doubled)  # [2, 4, 6, 8, 10]

# Convert to uppercase
names = ["alice", "bob", "charlie"]
upper_names = list(map(lambda name: name.upper(), names))
print(upper_names)  # ['ALICE', 'BOB', 'CHARLIE']
```

### Lambda with filter()

```python
# Filter even numbers
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
even = list(filter(lambda x: x % 2 == 0, numbers))
print(even)  # [2, 4, 6, 8, 10]

# Filter valid ports
ports = [80, 443, -1, 70000, 8080, 22]
valid_ports = list(filter(lambda p: 1 <= p <= 65535, ports))
print(valid_ports)  # [80, 443, 8080, 22]
```

---

## Security Examples

### 1. Port Validator

```python
def is_valid_port(port):
    """Check if port number is valid"""
    return 1 <= port <= 65535

def get_port_category(port):
    """Categorize port by range"""
    if not is_valid_port(port):
        return "Invalid"
    elif port < 1024:
        return "Well-known"
    elif port < 49152:
        return "Registered"
    else:
        return "Dynamic/Private"

# Test
ports = [22, 80, 443, 8080, 50000, 70000]
for port in ports:
    if is_valid_port(port):
        category = get_port_category(port)
        print(f"Port {port}: {category}")
    else:
        print(f"Port {port}: Invalid")
```

### 2. Password Strength Checker

```python
def check_password_strength(password):
    """
    Returns password strength score (0-100)
    """
    score = 0
    
    # Length check
    if len(password) >= 8:
        score += 25
    if len(password) >= 12:
        score += 10
    if len(password) >= 16:
        score += 15
    
    # Character variety
    if any(c.isupper() for c in password):
        score += 15
    if any(c.islower() for c in password):
        score += 15
    if any(c.isdigit() for c in password):
        score += 10
    if any(c in "!@#$%^&*()_+-=[]{}|;:',.<>?/" for c in password):
        score += 10
    
    return min(score, 100)

def get_strength_label(score):
    """Convert score to label"""
    if score < 40:
        return "❌ Weak"
    elif score < 70:
        return "⚠️ Medium"
    else:
        return "✅ Strong"

# Test passwords
passwords = ["pass", "Password1", "MyS3cure!Pass2024"]
for pwd in passwords:
    score = check_password_strength(pwd)
    label = get_strength_label(score)
    print(f"{pwd:20} - Score: {score:3}/100 - {label}")
```

### 3. Log Parser

```python
def parse_log_line(line):
    """Parse a log line and return dictionary"""
    parts = line.split(" - ")
    if len(parts) != 4:
        return None
    
    return {
        "timestamp": parts[0],
        "level": parts[1],
        "ip": parts[2],
        "message": parts[3]
    }

def filter_failed_logins(logs):
    """Extract failed login attempts"""
    failed = []
    for log in logs:
        parsed = parse_log_line(log)
        if parsed and "failed" in parsed["message"].lower():
            failed.append(parsed)
    return failed

def count_by_ip(log_entries):
    """Count occurrences by IP"""
    ip_counts = {}
    for entry in log_entries:
        ip = entry["ip"]
        ip_counts[ip] = ip_counts.get(ip, 0) + 1
    return ip_counts

# Test
logs = [
    "2024-01-15 10:30:00 - ERROR - 192.0.2.50 - Failed login attempt",
    "2024-01-15 10:31:15 - INFO - 192.0.2.51 - Successful login",
    "2024-01-15 10:32:30 - ERROR - 192.0.2.50 - Failed login attempt",
    "2024-01-15 10:35:45 - ERROR - 192.0.2.50 - Failed login attempt"
]

failed = filter_failed_logins(logs)
print(f"Failed logins: {len(failed)}")

ip_counts = count_by_ip(failed)
print("\nFailed attempts by IP:")
for ip, count in sorted(ip_counts.items(), key=lambda x: x[1], reverse=True):
    print(f"  {ip}: {count} attempts")
```

### 4. API Request Builder

```python
def build_api_url(base_url, endpoint, **params):
    """Build API URL with query parameters"""
    url = f"{base_url}/{endpoint}"
    
    if params:
        query_string = "&".join([f"{k}={v}" for k, v in params.items()])
        url += f"?{query_string}"
    
    return url

def make_api_request(url, api_key, **headers):
    """Simulate API request (in real code, use requests library)"""
    all_headers = {
        "Authorization": f"Bearer {api_key}",
        "Content-Type": "application/json"
    }
    all_headers.update(headers)
    
    print(f"URL: {url}")
    print(f"Headers: {all_headers}")
    return {"status": "success", "data": {}}

# Usage
base = "https://api.example.com"
url = build_api_url(base, "users", page=1, limit=10, sort="name")
make_api_request(url, "dummy_api_key_123", User_Agent="SecurityScanner/1.0")
```

---

## Best Practices

1. **Use descriptive names**: `validate_ip()` not `check()`
2. **One task per function**: Follow Single Responsibility Principle
3. **Keep functions short**: Ideally under 20 lines
4. **Add docstrings**: Document what the function does
5. **Return early**: Check errors first, return early
6. **Avoid side effects**: Functions should not modify global state

```python
def validate_ip_address(ip):
    """
    Validate IPv4 address format.
    
    Args:
        ip (str): IP address to validate
    
    Returns:
        bool: True if valid, False otherwise
    """
    # Early return for invalid input
    if not isinstance(ip, str):
        return False
    
    octets = ip.split(".")
    
    if len(octets) != 4:
        return False
    
    for octet in octets:
        if not octet.isdigit():
            return False
        num = int(octet)
        if num < 0 or num > 255:
            return False
    
    return True
```

---

## Practice Exercises

1. Create a function `scan_port_range(ip, start_port, end_port)` that prints all ports in range
2. Write `is_private_ip(ip)` to check if an IP is in private ranges (10.x.x.x, 192.168.x.x, 172.16-31.x.x)
3. Build `calculate_cidr(ip, subnet_mask)` to compute CIDR notation
4. Create `hash_password(password, *args, **kwargs)` that demonstrates args and kwargs

---

## See Also

- [Iteration](iteration.md) - Loops and iteration
- [Map, Lambda, Zip & Filters](functional-programming.md) - Functional programming
- [Dictionaries](dictionaries.md) - Working with data
