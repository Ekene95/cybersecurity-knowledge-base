# Python & APIs

Learn to interact with web APIs - essential for security testing, data collection, and automation.

---

## 📚 Table of Contents

- [What is an API?](#what-is-an-api)
- [HTTP Basics](#http-basics)
- [The requests Library](#the-requests-library)
- [Working with JSON](#working-with-json)
- [API Parameters](#api-parameters)
- [Response Handling](#response-handling)
- [Security Testing with APIs](#security-testing-with-apis)

---

## What is an API?

**API (Application Programming Interface)** is a way for programs to communicate with each other. Web APIs allow you to:
- Fetch data from servers
- Send data to servers
- Interact with web services programmatically

**Why APIs Matter for Security:**
- 🔍 Test web application endpoints
- 📊 Collect threat intelligence
- 🤖 Automate security tools
- 📡 Query vulnerability databases
- 🔐 Test authentication mechanisms

---

## HTTP Basics

HTTP (HyperText Transfer Protocol) is how web browsers and APIs communicate.

### HTTP Methods

| Method | Purpose | Example Use |
|--------|---------|-------------|
| `GET` | Retrieve data | Get user profile |
| `POST` | Send data | Create new user |
| `PUT` | Update data | Update user info |
| `DELETE` | Remove data | Delete user |
| `PATCH` | Partial update | Update email only |

### HTTP Status Codes

| Code | Meaning | Example |
|------|---------|---------|
| 200 | OK - Success | Request successful |
| 201 | Created | Resource created |
| 400 | Bad Request | Invalid data sent |
| 401 | Unauthorized | Need authentication |
| 403 | Forbidden | No permission |
| 404 | Not Found | Resource doesn't exist |
| 500 | Server Error | Server crashed |

---

## The requests Library

Python's `requests` library makes API calls easy.

### Installation

```bash
pip install requests
```

### Basic GET Request

```python
import requests

# Make a GET request
response = requests.get("https://api.open-notify.org/astros.json")

# Print response content
print(response.content)

# Print status code
print(f"Status code: {response.status_code}")
```

### Anatomy of a Request

```python
import requests

# URL to request
url = "https://api.example.com/users"

# Make request
response = requests.get(url)

# Useful response attributes
print(f"Status Code: {response.status_code}")  # 200
print(f"Headers: {response.headers}")          # Response headers
print(f"Content: {response.content}")          # Raw bytes
print(f"Text: {response.text}")                # Content as string
print(f"JSON: {response.json()}")              # Parse JSON to dict
```

---

## Working with JSON

APIs typically return data in JSON format. Python's `json` module makes it easy to work with.

### JSON Basics

```python
import json

# Python to JSON
data = {
    "username": "alice",
    "role": "admin",
    "active": True
}

# Convert dict to JSON string
json_string = json.dumps(data)
print(json_string)  # '{"username": "alice", "role": "admin", "active": true}'
print(type(json_string))  # <class 'str'>

# Convert JSON string to dict
data_back = json.loads(json_string)
print(data_back)  # {'username': 'alice', 'role': 'admin', 'active': True}
print(type(data_back))  # <class 'dict'>
```

### Parsing API Responses

```python
import requests

# Get data from API
response = requests.get("https://api.open-notify.org/astros.json")

# Parse JSON response
data = response.json()

# Access data like a dictionary
print(f"Number of people in space: {data['number']}")
print("\nAstronauts:")
for person in data['people']:
    print(f"  - {person['name']} on {person['craft']}")
```

### Pretty Printing JSON

```python
import json
import requests

response = requests.get("https://api.open-notify.org/astros.json")
data = response.json()

# Pretty print with indentation
print(json.dumps(data, indent=2))
```

---

## API Parameters

### Query Parameters (GET)

Query parameters are added to the URL after a `?`.

```python
import requests

# Method 1: Build URL manually
url = "http://api.open-notify.org/iss-pass.json?lat=37.78&lon=-122.41"
response = requests.get(url)

# Method 2: Use params dictionary (better!)
url = "http://api.open-notify.org/iss-pass.json"
parameters = {
    "lat": 37.78,
    "lon": -122.41
}
response = requests.get(url, params=parameters)

print(response.url)  # See final URL
print(response.json())
```

### Request Body (POST)

Send data in the request body:

```python
import requests

url = "https://httpbin.org/post"

# Send data as JSON
data = {
    "username": "alice",
    "email": "alice@example.com"
}

response = requests.post(url, json=data)
print(response.json())

# Or send as form data
form_data = {
    "username": "alice",
    "password": "dummy_pass_123"
}
response = requests.post(url, data=form_data)
```

---

## Response Handling

### Check Status Code

```python
import requests

response = requests.get("https://api.example.com/users")

if response.status_code == 200:
    print("✅ Success!")
    data = response.json()
elif response.status_code == 404:
    print("❌ Not found")
elif response.status_code >= 500:
    print("⚠️ Server error")
else:
    print(f"Unexpected status: {response.status_code}")
```

### Error Handling

```python
import requests

try:
    response = requests.get("https://api.example.com/data", timeout=5)
    response.raise_for_status()  # Raise exception for 4xx/5xx
    
    data = response.json()
    print(f"Data retrieved: {len(data)} items")
    
except requests.exceptions.Timeout:
    print("⏱️ Request timed out")
except requests.exceptions.ConnectionError:
    print("🔌 Connection error")
except requests.exceptions.HTTPError as e:
    print(f"❌ HTTP error: {e}")
except Exception as e:
    print(f"❌ Error: {e}")
```

### Response Headers

```python
import requests

response = requests.get("https://api.github.com")

# View all headers
print(response.headers)

# Access specific headers
print(f"Content-Type: {response.headers['Content-Type']}")
print(f"Server: {response.headers.get('Server', 'Unknown')}")
```

---

## Security Testing with APIs

### 1. API Endpoint Discovery

```python
import requests

base_url = "https://api.example.com"
endpoints = [
    "/users",
    "/admin",
    "/api/v1/config",
    "/api/v2/users",
    "/.git/config",
    "/backup.sql"
]

print("Testing endpoints...")
for endpoint in endpoints:
    url = base_url + endpoint
    response = requests.get(url)
    
    if response.status_code == 200:
        print(f"✅ Found: {endpoint} (Status: {response.status_code})")
    elif response.status_code == 403:
        print(f"🔒 Forbidden: {endpoint}")
    elif response.status_code == 401:
        print(f"🔐 Auth Required: {endpoint}")
    else:
        print(f"❌ Not Found: {endpoint}")
```

### 2. Rate Limit Testing

```python
import requests
import time

url = "https://api.example.com/search"

for i in range(1, 11):
    response = requests.get(url)
    
    print(f"Request {i}: Status {response.status_code}")
    
    # Check for rate limiting
    if response.status_code == 429:
        print("⚠️ Rate limit hit!")
        # Check Retry-After header
        retry_after = response.headers.get('Retry-After', 60)
        print(f"Retry after {retry_after} seconds")
        break
    
    time.sleep(0.5)  # Small delay between requests
```

### 3. Parameter Fuzzing

```python
import requests

url = "https://api.example.com/user"

# Test different parameter  values
test_values = [
    {"id": "1"},                    # Normal
    {"id": "999999"},               # Large number
    {"id": "-1"},                   # Negative
    {"id": "' OR '1'='1"},          # SQL injection
    {"id": "<script>alert(1)</script>"},  # XSS
    {"id": "../../../etc/passwd"},  # Path traversal
]

for params in test_values:
    response = requests.get(url, params=params)
    
    print(f"Testing {params}:")
    print(f"  Status: {response.status_code}")
    
    # Check response for errors or unexpected behavior
    if "error" in response.text.lower():
        print(f"  ⚠️ Error in response")
    if response.status_code == 500:
        print(f"  🔴 Server error - possible vulnerability!")
```

### 4. API Authentication Testing

```python
import requests

url = "https://api.example.com/protected"

# Test without authentication
response = requests.get(url)
print(f"No auth: {response.status_code}")  # Should be 401

# Test with invalid token
headers = {"Authorization": "Bearer invalid_token_123"}
response = requests.get(url, headers=headers)
print(f"Invalid token: {response.status_code}")  # Should be 401

# Test with valid token
headers = {"Authorization": "Bearer valid_dummy_token_abc123"}
response = requests.get(url, headers=headers)
print(f"Valid token: {response.status_code}")  # Should be 200
```

### 5. Data Extraction from API

```python
import requests
import json

# Example: Get vulnerability data (simulated)
url = "https://services.nvd.nist.gov/rest/json/cves/1.0"
params = {
    "keyword": "python",
    "resultsPerPage": 5
}

response = requests.get(url, params=params)

if response.status_code == 200:
    data = response.json()
    
    # Extract CVE IDs
    cves = data.get('result', {}).get('CVE_Items', [])
    
    print(f"Found {len(cves)} vulnerabilities:")
    for cve in cves:
        cve_id = cve['cve']['CVE_data_meta']['ID']
        description = cve['cve']['description']['description_data'][0]['value']
        print(f"\n{cve_id}:")
        print(f"  {description[:100]}...")
```

---

## Real-World Example: GitHub API

```python
import requests

# GitHub user info
username = "torvalds"
url = f"https://api.github.com/users/{username}"

response = requests.get(url)

if response.status_code == 200:
    user = response.json()
    
    print(f"Name: {user['name']}")
    print(f"Bio: {user['bio']}")
    print(f"Public Repos: {user['public_repos']}")
    print(f"Followers: {user['followers']}")
    print(f"Following: {user['following']}")
else:
    print(f"Error: {response.status_code}")
```

---

## Best Practices

1. **Always check status codes**: Don't assume success
2. **Use timeouts**: `requests.get(url, timeout=10)`
3. **Handle errors**: Use try/except blocks
4. **Respect rate limits**: Add delays between requests
5. **Never hardcode credentials**: Use environment variables
6. **Validate responses**: Check data structure before using

```python
import os
import requests

# Good practice
API_KEY = os.getenv("API_KEY", "default_dummy_key")

headers = {
    "Authorization": f"Bearer {API_KEY}"
}

try:
    response = requests.get(
        "https://api.example.com/data",
        headers=headers,
        timeout=10
    )
    response.raise_for_status()
    
    data = response.json()
    
    # Validate response structure
    if "results" in data:
        print(f"Found {len(data['results'])} results")
    else:
        print("Unexpected response format")
        
except Exception as e:
    print(f"Error: {e}")
```

---

## Practice Exercises

1. **Weather API**: Get current weather for your city using OpenWeatherMap API
2. **IP Geolocation**: Use IP-API.com to get location data for an IP address
3. **GitHub Repos**: List all repositories for a GitHub user
4. **API Fuzzer**: Build a simple fuzzer to test API endpoints

---

## Useful Free APIs for Practice

- **JSONPlaceholder**: https://jsonplaceholder.typicode.com/
- **Open Notify (ISS)**: http://api.open-notify.org/
- **HTTPBin**: https://httpbin.org/
- **IP-API**: https://ip-api.com/
- **GitHub**: https://api.github.com/

---

## See Also

- [API Authentication](api-authentication.md) - Secure API access
- [Dictionaries](dictionaries.md) - Working with JSON data
- [Functions](functions.md) - Organize API code
