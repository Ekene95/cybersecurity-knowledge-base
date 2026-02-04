# API Authentication

Master API authentication methods - essential for secure access to web services and security testing.

---

## 📚 Table of Contents

- [Why Authentication?](#why-authentication)
- [API Keys](#api-keys)
- [Bearer Tokens](#bearer-tokens)
- [Basic Authentication](#basic-authentication)
- [OAuth 2.0](#oauth-20)
- [Custom Headers](#custom-headers)
- [Security Best Practices](#security-best-practices)

---

## Why Authentication?

APIs use authentication to:
- 🔐 **Verify identity** - Confirm who is making the request
- 🛡️ **Authorize access** - Control what users can do
- 📊 **Track usage** - Monitor and limit API calls
- 🔒 **Protect data** - Prevent unauthorized access

**Common in Security Testing:**
- Testing authentication bypass
- Checking token security
- Verifying access controls
- Identifying privilege escalation

---

## API Keys

API keys are simple strings that identify your application.

### How API Keys Work

```python
import requests

# API key usually passed as query parameter or header
api_key = "dummy_api_key_abc123xyz789"

# Method 1: Query parameter
url = f"https://api.example.com/data?api_key={api_key}"
response = requests.get(url)

# Method 2: Header (more secure)
headers = {"X-API-Key": api_key}
response = requests.get("https://api.example.com/data", headers=headers)

print(f"Status: {response.status_code}")
```

### Real-World Example (OpenWeatherMap style)

```python
import requests
import os

# Get API key from environment variable
API_KEY = os.getenv("WEATHER_API_KEY", "demo_key_12345")

city = "London"
url = f"https://api.openweathermap.org/data/2.5/weather"

params = {
    "q": city,
    "appid": API_KEY,
    "units": "metric"
}

response = requests.get(url, params=params)

if response.status_code == 200:
    data = response.json()
    print(f"Temperature in {city}: {data['main']['temp']}°C")
elif response.status_code == 401:
    print("❌ Invalid API key")
else:
    print(f"Error: {response.status_code}")
```

### API Key Security

```python
import os

# ❌ Bad - Never hardcode API keys!
api_key = "sk-eAhAqPebdh6AHZodlHid"  # DON'T DO THIS

# ✅ Good - Use environment variables
api_key = os.getenv("API_KEY")

if not api_key:
    raise ValueError("API_KEY environment variable not set")

# ✅ Better - Use config file (not in git)
import json

with open("config.json") as f:
    config = json.load(f)
    api_key = config["api_key"]
```

---

## Bearer Tokens

Bearer tokens (often JWT) are used for more secure authentication.

### Basic Bearer Token Usage

```python
import requests

# Get token (simplified - usually from login endpoint)
token = "dummy_bearer_token_xyz789abc123"

# Add to Authorization header
headers = {
    "Authorization": f"Bearer {token}"
}

response = requests.get("https://api.example.com/protected", headers=headers)

if response.status_code == 200:
    print("✅ Authenticated successfully")
    data = response.json()
elif response.status_code == 401:
    print("❌ Invalid or expired token")
elif response.status_code == 403:
    print("🔒 Forbidden - insufficient permissions")
```

### GitHub API Example

```python
import requests
import os

# GitHub Personal Access Token
github_token = os.getenv("GITHUB_TOKEN", "ghp_dummyTokenABC123XYZ")

headers = {
    "Authorization": f"token {github_token}",  # GitHub uses 'token' prefix
    "Accept": "application/vnd.github.v3+json"
}

# Get authenticated user info
response = requests.get("https://api.github.com/user", headers=headers)

if response.status_code == 200:
    user = response.json()
    print(f"Logged in as: {user['login']}")
    print(f"Name: {user['name']}")
else:
    print(f"Authentication failed: {response.status_code}")
```

### Token Expiration Handling

```python
import requests
import time

class APIClient:
    def __init__(self, token):
        self.token = token
        self.token_expires = time.time() + 3600  # 1 hour
    
    def is_token_expired(self):
        return time.time() > self.token_expires
    
    def refresh_token(self):
        """Refresh the access token"""
        # In real code, call refresh endpoint
        print("🔄 Refreshing token...")
        self.token = "new_dummy_token_xyz123"
        self.token_expires = time.time() + 3600
    
    def make_request(self, url):
        if self.is_token_expired():
            self.refresh_token()
        
        headers = {"Authorization": f"Bearer {self.token}"}
        return requests.get(url, headers=headers)

# Usage
client = APIClient("initial_token_abc123")
response = client.make_request("https://api.example.com/data")
```

---

## Basic Authentication

Basic Auth sends username and password with each request (base64 encoded).

### Basic Auth in Python

```python
import requests
from requests.auth import HTTPBasicAuth

username = "api_user"
password = "dummy_password_123"

# Method 1: Using HTTPBasicAuth
response = requests.get(
    "https://api.example.com/data",
    auth=HTTPBasicAuth(username, password)
)

# Method 2: Tuple shorthand (same as above)
response = requests.get(
    "https://api.example.com/data",
    auth=(username, password)
)

print(f"Status: {response.status_code}")
```

### Manual Basic Auth Header

```python
import requests
import base64

username = "api_user"
password = "dummy_password_123"

# Create credentials string
credentials = f"{username}:{password}"

# Encode to base64
encoded = base64.b64encode(credentials.encode()).decode()

# Add to header
headers = {
    "Authorization": f"Basic {encoded}"
}

response = requests.get("https://api.example.com/data", headers=headers)
```

**⚠️ Security Note:** Basic Auth should only be used over HTTPS!

---

## OAuth 2.0

OAuth 2.0 is the industry standard for secure API authorization.

### OAuth Flow Overview

1. **Application** requests permission from user
2. **User** authorizes application
3. **Server** provides access token
4. **Application** uses token to access API

### Simplified OAuth Example

```python
import requests

# Step 1: Redirect user to authorization URL
auth_url = "https://oauth.example.com/authorize"
params = {
    "client_id": "dummy_client_id_123",
    "redirect_uri": "https://yourapp.com/callback",
    "response_type": "code",
    "scope": "read write"
}

# User visits this URL and authorizes
authorization_url = auth_url + "?" + "&".join([f"{k}={v}" for k, v in params.items()])
print(f"Visit: {authorization_url}")

# Step 2: After user authorizes, you get an authorization code
auth_code = "dummy_auth_code_xyz789"

# Step 3: Exchange code for access token
token_url = "https://oauth.example.com/token"
data = {
    "grant_type": "authorization_code",
    "code": auth_code,
    "client_id": "dummy_client_id_123",
    "client_secret": "dummy_client_secret_abc456",
    "redirect_uri": "https://yourapp.com/callback"
}

response = requests.post(token_url, data=data)
tokens = response.json()

access_token = tokens.get("access_token")
refresh_token = tokens.get("refresh_token")

# Step 4: Use access token
headers = {"Authorization": f"Bearer {access_token}"}
response = requests.get("https://api.example.com/user/data", headers=headers)
```

---

## Custom Headers

Some APIs use custom authentication headers.

### Custom Header Examples

```python
import requests

# Example 1: Custom API key header
headers = {
    "X-Custom-Auth": "dummy_custom_key_123",
    "X-Client-ID": "client_456"
}

response = requests.get("https://api.example.com/data", headers=headers)

# Example 2: Multiple authentication methods
headers = {
    "X-API-Key": "api_key_abc123",
    "X-Client-Token": "client_token_xyz789",
    "X-Request-ID": "req_12345"
}

response = requests.post(
    "https://api.example.com/secure",
    headers=headers,
    json={"action": "scan"}
)

# Example 3: Token with custom prefix
headers = {
    "Authorization": "Token abc123xyz789",  # Not 'Bearer'
    "X-API-Version": "2.0"
}

response = requests.get("https://api.example.com/v2/data", headers=headers)
```

---

## Security Best Practices

### 1. Never Hardcode Credentials

```python
import os

# ❌ BAD
api_key = "sk-eAhAqPebdh6AHZodlHid"

# ✅ GOOD
api_key = os.getenv("API_KEY")

# ✅ BETTER - with validation
api_key = os.getenv("API_KEY")
if not api_key:
    raise ValueError("API_KEY environment variable must be set")
```

### 2. Use Environment Variables

```bash
# Set in terminal
export API_KEY="your_key_here"
export API_SECRET="your_secret_here"

# Or use .env file (never commit to git!)
echo "API_KEY=your_key_here" > .env
echo "API_SECRET=your_secret_here" >> .env

# Add .env to .gitignore
echo ".env" >> .gitignore
```

```python
# Load from .env file
from dotenv import load_dotenv
import os

load_dotenv()  # Load from .env file

api_key = os.getenv("API_KEY")
api_secret = os.getenv("API_SECRET")
```

### 3. Validate SSL Certificates

```python
import requests

# ✅ Default - verify SSL (recommended)
response = requests.get("https://api.example.com/data")

# ❌ Dangerous - disable SSL verification (only for testing!)
response = requests.get("https://api.example.com/data", verify=False)

# ✅ Custom CA certificate
response = requests.get("https://api.example.com/data", verify="/path/to/ca-bundle.crt")
```

### 4. Implement Rate Limiting

```python
import requests
import time

class RateLimitedAPI:
    def __init__(self, token, requests_per_minute=60):
        self.token = token
        self.requests_per_minute = requests_per_minute
        self.min_interval = 60 / requests_per_minute
        self.last_request = 0
    
    def make_request(self, url):
        # Wait if needed to respect rate limit
        elapsed = time.time() - self.last_request
        if elapsed < self.min_interval:
            time.sleep(self.min_interval - elapsed)
        
        headers = {"Authorization": f"Bearer {self.token}"}
        response = requests.get(url, headers=headers)
        
        self.last_request = time.time()
        return response

# Usage
api = RateLimitedAPI("dummy_token_123", requests_per_minute=30)
response = api.make_request("https://api.example.com/data")
```

### 5. Handle Authentication Errors

```python
import requests

def make_authenticated_request(url, token):
    headers = {"Authorization": f"Bearer {token}"}
    
    try:
        response = requests.get(url, headers=headers, timeout=10)
        
        if response.status_code == 200:
            return response.json()
        elif response.status_code == 401:
            print("❌ Authentication failed - invalid token")
            return None
        elif response.status_code == 403:
            print("🔒 Access denied - insufficient permissions")
            return None
        elif response.status_code == 429:
            print("⏱️ Rate limit exceeded")
            retry_after = response.headers.get('Retry-After', 60)
            print(f"Retry after {retry_after} seconds")
            return None
        else:
            print(f"❌ Unexpected error: {response.status_code}")
            return None
            
    except requests.exceptions.Timeout:
        print("⏱️ Request timed out")
        return None
    except Exception as e:
        print(f"❌ Error: {e}")
        return None

# Usage
token = os.getenv("API_TOKEN", "dummy_token_123")
data = make_authenticated_request("https://api.example.com/data", token)
```

---

## Security Testing Scenarios

### 1. Test Authentication Bypass

```python
import requests

url = "https://api.example.com/admin/users"

# Test without authentication
print("Test 1: No authentication")
response = requests.get(url)
print(f"  Status: {response.status_code}")  # Should be 401

# Test with invalid token
print("\nTest 2: Invalid token")
headers = {"Authorization": "Bearer invalid_token"}
response = requests.get(url, headers=headers)
print(f"  Status: {response.status_code}")  # Should be 401

# Test with SQL injection in token
print("\nTest 3: SQL injection")
headers = {"Authorization": "Bearer ' OR '1'='1"}
response = requests.get(url, headers=headers)
print(f"  Status: {response.status_code}")  # Should be 401, not 200!
```

### 2. Token Privilege Escalation

```python
import requests

# Low-privilege token
user_token = "user_token_abc123"

# Try accessing admin endpoint
admin_url = "https://api.example.com/admin/config"
headers = {"Authorization": f"Bearer {user_token}"}

response = requests.get(admin_url, headers=headers)

if response.status_code == 200:
    print("🔴 VULNERABILITY: User token has admin access!")
elif response.status_code == 403:
    print("✅ Properly restricted")
else:
    print(f"Status: {response.status_code}")
```

### 3. API Key Leakage Detection

```python
import requests

# Check if API key is exposed in response
response = requests.get("https://api.example.com/config")

# Look for common API key patterns
import re

api_key_patterns = [
    r'api[_-]?key["\']?\s*[:=]\s*["\']([A-Za-z0-9-_]+)["\']',
    r'token["\']?\s*[:=]\s*["\']([A-Za-z0-9-_]+)["\']',
    r'sk-[A-Za-z0-9]{32,}',  # OpenAI style
]

for pattern in api_key_patterns:
    matches = re.findall(pattern, response.text)
    if matches:
        print(f"⚠️ Potential API key found: {pattern}")
        print(f"   Matches: {matches}")
```

---

## Complete Authentication Example

```python
import requests
import os
import json
from datetime import datetime, timedelta

class SecureAPIClient:
    def __init__(self):
        self.api_key = os.getenv("API_KEY")
        self.api_secret = os.getenv("API_SECRET")
        self.base_url = "https://api.example.com"
        self.token = None
        self.token_expires = None
    
    def authenticate(self):
        """Get access token"""
        auth_url = f"{self.base_url}/auth/token"
        
        response = requests.post(
            auth_url,
            json={
                "api_key": self.api_key,
                "api_secret": self.api_secret
            },
            timeout=10
        )
        
        if response.status_code == 200:
            data = response.json()
            self.token = data["access_token"]
            expires_in = data.get("expires_in", 3600)
            self.token_expires = datetime.now() + timedelta(seconds=expires_in)
            print("✅ Authenticated successfully")
            return True
        else:
            print(f"❌ Authentication failed: {response.status_code}")
            return False
    
    def is_token_valid(self):
        """Check if token is still valid"""
        if not self.token:
            return False
        if datetime.now() >= self.token_expires:
            return False
        return True
    
    def make_request(self, endpoint, method="GET", data=None):
        """Make authenticated API request"""
        # Ensure we have valid token
        if not self.is_token_valid():
            if not self.authenticate():
                return None
        
        url = f"{self.base_url}/{endpoint}"
        headers = {
            "Authorization": f"Bearer {self.token}",
            "Content-Type": "application/json"
        }
        
        try:
            if method == "GET":
                response = requests.get(url, headers=headers, timeout=10)
            elif method == "POST":
                response = requests.post(url, headers=headers, json=data, timeout=10)
            else:
                raise ValueError(f"Unsupported method: {method}")
            
            response.raise_for_status()
            return response.json()
            
        except requests.exceptions.HTTPError as e:
            print(f"HTTP Error: {e}")
            return None
        except Exception as e:
            print(f"Error: {e}")
            return None

# Usage
client = SecureAPIClient()
data = client.make_request("users")
if data:
    print(f"Retrieved {len(data)} users")
```

---

## Practice Exercises

1. Create a function that tests different authentication methods for an API
2. Build a token rotation system that refreshes before expiration
3. Implement rate limiting based on API response headers
4. Create an authentication bypass tester

---

## See Also

- [Python & APIs](python-and-apis.md) - API basics
- [Functions](functions.md) - Organize authentication code
- [Dictionaries](dictionaries.md) - Handle API responses
