# Python Dictionaries

Master Python dictionaries - the most important data structure for storing configuration, parsing JSON APIs, and organizing security data.

---

## 📚 Table of Contents

- [What is a Dictionary?](#what-is-a-dictionary)
- [Creating Dictionaries](#creating-dictionaries)
- [Accessing Data](#accessing-data)
- [Modifying Dictionaries](#modifying-dictionaries)
- [Dictionary Methods](#dictionary-methods)
- [Nested Dictionaries](#nested-dictionaries)
- [Security Use Cases](#security-use-cases)

---

## What is a Dictionary?

A dictionary is a collection of **key-value pairs**, like a real dictionary where you look up a word (key) to find its definition (value).

```python
# Simple dictionary
user = {
    "username": "alice",
    "role": "admin",
    "age": 25
}
```

**Why dictionaries matter for security:**
- 📊 Store vulnerability scan results
- 🔑 Manage API keys and tokens
- 📝 Parse JSON responses from APIs
- ⚙️ Configure security tools
- 📈 Track statistics and metrics

---

## Creating Dictionaries

### Basic Creation

```python
# Empty dictionary
config = {}
config = dict()

# With initial data
user = {
    "name": "Alice",
    "email": "alice@example.com",
    "is_admin": True
}

# Using dict() constructor
user = dict(name="Alice", email="alice@example.com", is_admin=True)
```

### Real Security Examples

```python
# Scan configuration
scan_config = {
    "target": "192.0.2.10",
    "ports": [80, 443, 8080],
    "timeout": 30,
    "threads": 10
}

# Vulnerability findings
vuln = {
    "id": "CVE-2024-1234",
    "severity": "HIGH",
    "affected_version": "1.2.3",
    "patched": False
}

# API credentials (never hardcode in real code!)
api_creds = {
    "api_key": "dummy_key_12345",
    "api_secret": "dummy_secret_67890",
    "endpoint": "https://api.example.com"
}
```

---

## Accessing Data

### Basic Access

```python
user = {
    "username": "alice",
    "role": "admin",
    "age": 25
}

# Access by key
print(user["username"])  # 'alice'
print(user["role"])      # 'admin'

# Key error if not found
try:
    print(user["email"])  # KeyError!
except KeyError:
    print("❌ Key not found")
```

### Safe Access with get()

```python
user = {"username": "alice", "role": "admin"}

# get() returns None if key doesn't exist
email = user.get("email")  # None
print(email)

# Provide default value
email = user.get("email", "not.found@example.com")
print(email)  # 'not_found@example.com'

# Check if key exists
if "role" in user:
    print(f"Role: {user['role']}")
```

**Security Example:**
```python
# Safely get configuration with defaults
scan_config = {"target": "192.0.2.10", "timeout": 30}

ports = scan_config.get("ports", [80, 443])  # Default ports
threads = scan_config.get("threads", 1)       # Default 1 thread
timeout = scan_config.get("timeout", 60)      # From config: 30

print(f"Scanning {scan_config['target']} with {threads} threads")
```

---

## Modifying Dictionaries

### Adding & Updating

```python
user = {"username": "alice"}

# Add new key
user["role"] = "admin"
user["age"] = 25

# Update existing key
user["age"] = 26

print(user)  # {'username': 'alice', 'role': 'admin', 'age': 26}

# Update multiple keys
user.update({"email": "alice@example.com", "active": True})
```

### Removing Items

```python
user = {
    "username": "alice",
    "role": "admin",
    "temp_token": "abc123"
}

# Remove specific key with del
del user["temp_token"]

# Remove and return value with pop()
role = user.pop("role")  # Returns 'admin' and removes key
print(role)

# Pop with default (if key doesn't exist)
email = user.pop("email", "none@example.com")

# Remove all items
user.clear()
print(user)  # {}
```

**Security Example:**
```python
# Track failed login attempts
failed_logins = {}

ip = "192.0.2.50"

# Increment counter
if ip in failed_logins:
    failed_logins[ip] += 1
else:
    failed_logins[ip] = 1

# Remove if threshold exceeded (ban IP)
if failed_logins[ip] > 5:
    print(f"🔒 IP {ip} banned after {failed_logins[ip]} attempts")
    del failed_logins[ip]
```

---

## Dictionary Methods

### Keys, Values, and Items

```python
user = {
    "username": "alice",
    "role": "admin",
    "age": 25
}

# Get all keys
keys = user.keys()
print(list(keys))  # ['username', 'role', 'age']

# Get all values
values = user.values()
print(list(values))  # ['alice', 'admin', 25]

# Get key-value pairs (tuples)
items = user.items()
print(list(items))  # [('username', 'alice'), ('role', 'admin'), ('age', 25)]

# Iterate over dictionary
for key in user:
    print(f"{key}: {user[key]}")

for key, value in user.items():
    print(f"{key} = {value}")
```

### Sorting Dictionaries

```python
# Dictionary is unordered by default (pre-Python 3.7)
# In Python 3.7+, dictionaries maintain insertion order

scores = {"alice": 95, "bob": 87, "charlie": 92}

# Sort by keys
sorted_dict = dict(sorted(scores.items()))
print(sorted_dict)  # {'alice': 95, 'bob': 87, 'charlie': 92}

# Sort by values
sorted_by_value = dict(sorted(scores.items(), key=lambda x: x[1]))
print(sorted_by_value)  # {'bob': 87, 'charlie': 92, 'alice': 95}

# Sort by values (descending)
sorted_desc = dict(sorted(scores.items(), key=lambda x: x[1], reverse=True))
print(sorted_desc)  # {'alice': 95, 'charlie': 92, 'bob': 87}
```

**Security Example:**
```python
# Sort vulnerabilities by severity score
vulns = {
    "XSS": 7.5,
    "SQLi": 9.8,
    "CSRF": 6.1,
    "RCE": 10.0
}

# Sort by severity (highest first)
sorted_vulns = dict(sorted(vulns.items(), key=lambda x: x[1], reverse=True))

print("Vulnerabilities by severity:")
for vuln, score in sorted_vulns.items():
    print(f"  {score:4.1f} - {vuln}")
```

---

## Nested Dictionaries

Dictionaries can contain other dictionaries - perfect for complex data structures.

### Basic Nested Structure

```python
users = {
    "alice": {
        "role": "admin",
        "email": "alice@example.com",
        "permissions": ["read", "write", "delete"]
    },
    "bob": {
        "role": "user",
        "email": "bob@example.com",
        "permissions": ["read"]
    }
}

# Access nested data
print(users["alice"]["role"])  # 'admin'
print(users["bob"]["permissions"])  # ['read']

# Iterate nested dictionary
for username, details in users.items():
    print(f"User: {username}")
    print(f"  Role: {details['role']}")
    print(f"  Email: {details['email']}")
```

### Real Security Data Structure

```python
scan_results = {
    "192.0.2.10": {
        "hostname": "web-server-1",
        "open_ports": [80, 443, 22],
        "services": {
            80: "nginx 1.18",
            443: "nginx 1.18",
            22: "OpenSSH 8.2"
        },
        "vulnerabilities": [
            {"id": "CVE-2024-1234", "severity": "HIGH"},
            {"id": "CVE-2024-5678", "severity": "MEDIUM"}
        ]
    },
    "192.0.2.11": {
        "hostname": "db-server-1",
        "open_ports": [3306, 22],
        "services": {
            3306: "MySQL 8.0",
            22: "OpenSSH 8.2"
        },
        "vulnerabilities": []
    }
}

# Access specific data
web_server = scan_results["192.0.2.10"]
print(f"Hostname: {web_server['hostname']}")
print(f"Open ports: {web_server['open_ports']}")
print(f"Web server: {web_server['services'][80]}")

# Check for vulnerabilities
for ip, details in scan_results.items():
    vuln_count = len(details['vulnerabilities'])
    if vuln_count > 0:
        print(f"⚠️ {ip} has {vuln_count} vulnerabilities")
    else:
        print(f"✅ {ip} - No vulnerabilities found")
```

---

## Security Use Cases

### 1. Parsing JSON API Response

```python
import json

# Simulated API response (JSON string)
api_response = '''
{
    "status": "success",
    "data": {
        "user_count": 150,
        "active_sessions": 45,
        "failed_logins": 12
    },
    "timestamp": "2024-01-15T10:30:00Z"
}
'''

# Parse JSON to dictionary
response_dict = json.loads(api_response)

# Access data
print(f"Status: {response_dict['status']}")
print(f"Active sessions: {response_dict['data']['active_sessions']}")
print(f"Failed logins: {response_dict['data']['failed_logins']}")

# Convert dictionary to JSON
user_data = {"username": "alice", "role": "admin"}
json_string = json.dumps(user_data)
print(json_string)  # '{"username": "alice", "role": "admin"}'
```

### 2. Configuration Manager

```python
# Security tool configuration
config = {
    "scan_targets": ["192.0.2.10", "192.0.2.11"],
    "scan_options": {
        "port_range": "1-1000",
        "timeout": 30,
        "threads": 10,
        "aggressive": False
    },
    "output": {
        "format": "json",
        "save_to": "/tmp/scan_results.json"
    },
    "notifications": {
        "email": "admin@example.com",
        "on_complete": True,
        "on_critical_vuln": True
    }
}

# Use configuration
for target in config["scan_targets"]:
    threads = config["scan_options"]["threads"]
    timeout = config["scan_options"]["timeout"]
    print(f"Scanning {target} with {threads} threads (timeout: {timeout}s)")

if config["notifications"]["on_complete"]:
    email = config["notifications"]["email"]
    print(f"Will send completion email to {email}")
```

### 3. Vulnerability Tracker

```python
vulnerabilities = {}

def add_vulnerability(id, severity, description):
    vulnerabilities[id] = {
        "severity": severity,
        "description": description,
        "discovered": "2024-01-15",
        "patched": False
    }

def mark_patched(id):
    if id in vulnerabilities:
        vulnerabilities[id]["patched"] = True
        print(f"✅ {id} marked as patched")

def get_critical_vulns():
    critical = {}
    for vuln_id, details in vulnerabilities.items():
        if details["severity"] == "CRITICAL" and not details["patched"]:
            critical[vuln_id] = details
    return critical

# Usage
add_vulnerability("CVE-2024-1234", "CRITICAL", "Remote Code Execution")
add_vulnerability("CVE-2024-5678", "HIGH", "SQL Injection")
add_vulnerability("CVE-2024-9999", "MEDIUM", "XSS")

mark_patched("CVE-2024-9999")

critical = get_critical_vulns()
print(f"\n⚠️ {len(critical)} critical vulnerabilities unpatched:")
for id, details in critical.items():
    print(f"  - {id}: {details['description']}")
```

### 4. IP Reputation Tracker

```python
ip_reputation = {}

def log_activity(ip, action):
    if ip not in ip_reputation:
        ip_reputation[ip] = {
            "failed_logins": 0,
            "successful_logins": 0,
            "blocked": False,
            "first_seen": "2024-01-15"
        }
    
    if action == "login_failed":
        ip_reputation[ip]["failed_logins"] += 1
        
        # Block if too many failures
        if ip_reputation[ip]["failed_logins"] >= 5:
            ip_reputation[ip]["blocked"] = True
            print(f"🔒 {ip} has been blocked")
    
    elif action == "login_success":
        ip_reputation[ip]["successful_logins"] += 1

def is_blocked(ip):
    return ip in ip_reputation and ip_reputation[ip]["blocked"]

def get_report():
    print("\n📊 IP Reputation Report")
    print("=" * 50)
    for ip, stats in ip_reputation.items():
        status = "🔒 BLOCKED" if stats["blocked"] else "✅ Active"
        print(f"{ip} - {status}")
        print(f"  Failed: {stats['failed_logins']}, Success: {stats['successful_logins']}")

# Usage
log_activity("192.0.2.50", "login_failed")
log_activity("192.0.2.50", "login_failed")
log_activity("192.0.2.50", "login_failed")
log_activity("192.0.2.50", "login_failed")
log_activity("192.0.2.50", "login_failed")
log_activity("192.0.2.51", "login_success")

print(f"Is 192.0.2.50 blocked? {is_blocked('192.0.2.50')}")
get_report()
```

---

## Dictionary Comprehension

Create dictionaries quickly with comprehension syntax.

```python
# Basic comprehension
squares = {x: x**2 for x in range(1, 6)}
print(squares)  # {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}

# With condition
even_squares = {x: x**2 for x in range(1, 11) if x % 2 == 0}
print(even_squares)  # {2: 4, 4: 16, 6: 36, 8: 64, 10: 100}

# From lists
ports = [80, 443, 8080, 22]
port_status = {port: "open" for port in ports}
print(port_status)  # {80: 'open', 443: 'open', 8080: 'open', 22: 'open'}

# Transform existing dictionary
scores = {"alice": 85, "bob": 92, "charlie": 78}
grades = {name: "PASS" if score >= 80 else "FAIL" for name, score in scores.items()}
print(grades)  # {'alice': 'PASS', 'bob': 'PASS', 'charlie': 'FAIL'}
```

---

## Practice Exercises

1. **User Management**: Create a dictionary to track user permissions and write functions to add/remove permissions

2. **Port Scanner Results**: Build a nested dictionary to store scan results for multiple IPs with open ports and services

3. **Vulnerability Database**: Create a system to track CVEs with functions to filter by severity

4. **API Response Parser**: Parse a complex JSON API response and extract specific fields

---

## Quick Reference

### Dictionary Methods

| Method | Purpose | Example |
|--------|---------|---------|
| `.get(key, default)` | Safe access | `d.get("age", 0)` |
| `.keys()` | Get all keys | `list(d.keys())` |
| `.values()` | Get all values | `list(d.values())` |
| `.items()` | Get pairs | `list(d.items())` |
| `.pop(key)` | Remove & return | `d.pop("age")` |
| `.update(dict2)` | Merge dicts | `d.update({"key": "val"})` |
| `.clear()` | Remove all | `d.clear()` |

---

## See Also

- [Strings & Variables](strings-and-variables.md) - Data basics
- [Functions](functions.md) - Organize code
- [Python & APIs](python-and-apis.md) - Work with JSON
