# Practice Scripts & Exercises

Hands-on coding challenges to master Python fundamentals - practice makes perfect!

---

## 📚 Table of Contents

- [How to Practice](#how-to-practice)
- [Beginner Exercises](#beginner-exercises)
- [Intermediate Challenges](#intermediate-challenges)
- [Advanced Projects](#advanced-projects)
- [Security-Focused Exercises](#security-focused-exercises)
- [Solutions & Tips](#solutions--tips)

---

## How to Practice

### Online Editors

- **PyNative**: https://pynative.com/online-python-code-editor-to-execute-python-code/
- **Replit**: https://replit.com/
- **Google Colab**: https://colab.research.google.com/

### Local Setup

```bash
# Create practice directory
mkdir python-practice
cd python-practice

# Create a test file
touch exercise1.py

# Run it
python3 exercise1.py
```

### Tips for Success

1. **Read the problem carefully**
2. **Start with pseudocode** - plan before coding
3. **Test with different inputs**
4. **Don't look at solutions immediately**
5. **Debug by printing variables**
6. **Refactor after it works**

---

## Beginner Exercises

### Exercise 1: Character ASCII Sum

**Problem:** Given a name, calculate the sum of ASCII values for each character.

```python
# Example:
# Input: "Josef"
# Output: Sum of ord('J') + ord('o') + ord('s') + ord('e') + ord('f')
```

**Hints:**
- Use `ord()` function to get ASCII value
- Use a loop to iterate through characters
- Keep a running total

<details>
<summary>Click to see solution</summary>

```python
name = "Josef"
total = 0

for char in name:
    total += ord(char)

print(f"ASCII sum of '{name}': {total}")

# Alternative: One-liner
total = sum(ord(char) for char in name)
print(f"ASCII sum (one-liner): {total}")
```
</details>

---

### Exercise 2: Sum Range of Numbers

**Problem:** Calculate the sum of all numbers between 1 and 1000.

**Hints:**
- Use `range()` function
- Use a loop or built-in `sum()`

<details>
<summary>Click to see solution</summary>

```python
# Method 1: Using loop
total = 0
for num in range(1, 1001):  # 1001 because range is exclusive
    total += num
print(f"Sum (loop): {total}")

# Method 2: Using built-in sum()
total = sum(range(1, 1001))
print(f"Sum (built-in): {total}")

# Method 3: Mathematical formula
n = 1000
total = n * (n + 1) // 2
print(f"Sum (formula): {total}")
```
</details>

---

### Exercise 3: Count Characters in List

**Problem:** Count the total number of characters in a list of strings.

```python
# Input:
lst = ['abcd', '123', 'xyz', 'john', 70.2]

# Expected output:
# Total characters: 15 (strings only)
```

**Hints:**
- Check if item is a string using `isinstance()`
- Use `len()` to get string length

<details>
<summary>Click to see solution</summary>

```python
lst = ['abcd', '123', 'xyz', 'john', 70.2]
total_chars = 0

for item in lst:
    if isinstance(item, str):
        total_chars += len(item)

print(f"Total characters: {total_chars}")

# Alternative: Using list comprehension
total = sum(len(item) for item in lst if isinstance(item, str))
print(f"Total (comprehension): {total}")
```
</details>

---

### Exercise 4: Sum ASCII Characters (a-z)

**Problem:** Calculate the sum of ASCII values for all lowercase letters (a-z).

**Hints:**
- ASCII values: a=97, b=98, ..., z=122
- Use `chr()` to get character from ASCII value
- Use `ord()` to get ASCII value from character

<details>
<summary>Click to see solution</summary>

```python
# Method 1: Using range
total = 0
for ascii_val in range(ord('a'), ord('z') + 1):
    total += ascii_val
print(f"Sum of a-z ASCII values: {total}")

# Method 2: Using string iteration
import string
total = sum(ord(char) for char in string.ascii_lowercase)
print(f"Sum (using string module): {total}")

# Method 3: Mathematical formula
# Sum = n*(first + last)/2, where n = 26
n = 26
first = ord('a')
last = ord('z')
total = n * (first + last) // 2
print(f"Sum (formula): {total}")
```
</details>

---

### Exercise 5: Square Numbers in List

**Problem:** Create a list of squares and calculate their sum.

```python
# Input:
lst = [3, 7, 6, 8, 9, 11, 15, 25]

# Expected:
# Create new list: [9, 49, 36, 64, 81, 121, 225, 625]
# Sum: 1210
```

<details>
<summary>Click to see solution</summary>

```python
lst = [3, 7, 6, 8, 9, 11, 15, 25]

# Create list of squares
squared = []
for num in lst:
    squared.append(num ** 2)

print(f"Squared list: {squared}")
print(f"Sum: {sum(squared)}")

# Alternative: List comprehension
squared = [num ** 2 for num in lst]
total = sum(squared)
print(f"Sum (comprehension): {total}")
```
</details>

---

### Exercise 6: Square String Numbers

**Problem:** Convert string numbers to integers, square them, and sum.

```python
# Input:
lst = ["4", "8", "7", "41", "12", "2", "15", "33"]

# Expected: Convert to int, square each, sum them
```

<details>
<summary>Click to see solution</summary>

```python
lst = ["4", "8", "7", "41", "12", "2", "15", "33"]

# Convert to int, square, and sum
squared = []
for num_str in lst:
    num = int(num_str)
    squared.append(num ** 2)

print(f"Squared: {squared}")
print(f"Sum: {sum(squared)}")

# One-liner
total = sum(int(num) ** 2 for num in lst)
print(f"Sum (one-liner): {total}")
```
</details>

---

### Exercise 7: Sum Multiples of Seven

**Problem:** Find the sum of all numbers divisible by 7 between 100 and 10000.

**Hints:**
- Use modulo operator `%`
- If `num % 7 == 0`, it's divisible by 7

<details>
<summary>Click to see solution</summary>

```python
# Method 1: Loop with condition
total = 0
for num in range(100, 10001):
    if num % 7 == 0:
        total += num

print(f"Sum of multiples of 7: {total}")

# Method 2: Using range step
total = sum(range(105, 10001, 7))  # Start at first multiple of 7
print(f"Sum (optimized): {total}")

# Method 3: List comprehension
total = sum(num for num in range(100, 10001) if num % 7 == 0)
print(f"Sum (comprehension): {total}")
```
</details>

---

## Intermediate Challenges

### Challenge 1: FizzBuzz

**Problem:** Print numbers 1-100, but:
- "Fizz" for multiples of 3
- "Buzz" for multiples of 5
- "FizzBuzz" for multiples of both

<details>
<summary>Click to see solution</summary>

```python
for num in range(1, 101):
    if num % 3 == 0 and num % 5 == 0:
        print("FizzBuzz")
    elif num % 3 == 0:
        print("Fizz")
    elif num % 5 == 0:
        print("Buzz")
    else:
        print(num)

# More elegant version
for num in range(1, 101):
    output = ""
    if num % 3 == 0:
        output += "Fizz"
    if num % 5 == 0:
        output += "Buzz"
    print(output or num)
```
</details>

---

### Challenge 2: Palindrome Checker

**Problem:** Check if a string reads the same forwards and backwards.

```python
# Examples:
# "radar" → True
# "hello" → False
# "A man a plan a canal Panama" → True (ignoring spaces, case)
```

<details>
<summary>Click to see solution</summary>

```python
def is_palindrome(text):
    # Clean text: remove spaces, convert to lowercase
    cleaned = text.replace(" ", "").lower()
    
    # Check if equal to reverse
    return cleaned == cleaned[::-1]

# Test
test_words = ["radar", "hello", "racecar", "python", "noon"]
for word in test_words:
    result = "✅" if is_palindrome(word) else "❌"
    print(f"{result} '{word}'")

# Test with phrase
phrase = "A man a plan a canal Panama"
print(f"\n'{phrase}': {is_palindrome(phrase)}")
```
</details>

---

### Challenge 3: Prime Number Finder

**Problem:** Find all prime numbers between 1 and 100.

**Hint:** A prime number is only divisible by 1 and itself.

<details>
<summary>Click to see solution</summary>

```python
def is_prime(n):
    """Check if number is prime"""
    if n < 2:
        return False
    for i in range(2, int(n ** 0.5) + 1):
        if n % i == 0:
            return False
    return True

# Find all primes up to 100
primes = []
for num in range(1, 101):
    if is_prime(num):
        primes.append(num)

print(f"Primes 1-100: {primes}")
print(f"Total primes: {len(primes)}")

# Using list comprehension
primes = [num for num in range(1, 101) if is_prime(num)]
print(f"Primes (comprehension): {primes}")
```
</details>

---

### Challenge 4: Fibonacci Sequence

**Problem:** Generate the first 20 numbers in the Fibonacci sequence.

**Hint:** Each number is the sum of the previous two: 0, 1, 1, 2, 3, 5, 8, 13...

<details>
<summary>Click to see solution</summary>

```python
def fibonacci(n):
    """Generate first n Fibonacci numbers"""
    fib = [0, 1]
    
    while len(fib) < n:
        next_num = fib[-1] + fib[-2]
        fib.append(next_num)
    
    return fib[:n]

# Generate first 20
fib_sequence = fibonacci(20)
print(f"First 20 Fibonacci numbers:")
print(fib_sequence)

# Alternative: Using loop
a, b = 0, 1
fib = []
for _ in range(20):
    fib.append(a)
    a, b = b, a + b
print(f"Fibonacci (loop): {fib}")
```
</details>

---

### Challenge 5: Character Frequency Counter

**Problem:** Count how many times each character appears in a string.

```python
# Input: "hello world"
# Output: {'h': 1, 'e': 1, 'l': 3, 'o': 2, 'w': 1, 'r': 1, 'd': 1}
```

<details>
<summary>Click to see solution</summary>

```python
def count_characters(text):
    """Count character frequency"""
    freq = {}
    
    for char in text:
        if char != ' ':  # Ignore spaces
            freq[char] = freq.get(char, 0) + 1
    
    return freq

# Test
text = "hello world"
frequency = count_characters(text)

print("Character frequency:")
for char, count in sorted(frequency.items()):
    print(f"  '{char}': {count}")

# Using Counter from collections
from collections import Counter
text = "hello world"
freq = Counter(text.replace(" ", ""))
print(f"\nUsing Counter: {dict(freq)}")
```
</details>

---

## Advanced Projects

### Project 1: Password Generator

**Problem:** Create a password generator with options for length and complexity.

```python
import random
import string

def generate_password(length=12, use_uppercase=True, use_digits=True, use_special=True):
    """Generate random password"""
    chars = string.ascii_lowercase
    
    if use_uppercase:
        chars += string.ascii_uppercase
    if use_digits:
        chars += string.digits
    if use_special:
        chars += "!@#$%^&*()-_=+"
    
    password = ''.join(random.choice(chars) for _ in range(length))
    return password

# Generate passwords
print("Generated passwords:")
for i in range(5):
    pwd = generate_password(16)
    print(f"  {i+1}. {pwd}")

# Custom options
weak_pwd = generate_password(8, use_uppercase=False, use_special=False)
strong_pwd = generate_password(20, use_uppercase=True, use_digits=True, use_special=True)

print(f"\nWeak: {weak_pwd}")
print(f"Strong: {strong_pwd}")
```

---

### Project 2: Simple Port Scanner

**Problem:** Simulate a port scanner (educational purposes only).

```python
import random

def scan_port(ip, port):
    """Simulate port scan (random result for demo)"""
    # In real code, you'd use socket module
    is_open = random.choice([True, False])
    return is_open

def scan_ports(ip, ports):
    """Scan multiple ports and return open ones"""
    open_ports = []
    
    print(f"Scanning {ip}...")
    for port in ports:
        is_open = scan_port(ip, port)
        status = "OPEN" if is_open else "CLOSED"
        print(f"  Port {port:5d}: {status}")
        
        if is_open:
            open_ports.append(port)
    
    return open_ports

# Scan common ports
target_ip = "192.0.2.10"
common-ports = [21, 22, 80, 443, 3306, 8080, 8443]

results = scan_ports(target_ip, common_ports)

print(f"\n{len(results)} ports open: {results}")
```

---

### Project 3: Log File Analyzer

**Problem:** Analyze log entries and extract insights.

```python
def parse_log_line(line):
    """Parse log line into components"""
    parts = line.split(" - ")
    return {
        "timestamp": parts[0],
        "level": parts[1].strip(),
        "ip": parts[2],
        "message": parts[3]
    }

def analyze_logs(log_lines):
    """Analyze logs and return statistics"""
    stats = {
        "total": len(log_lines),
        "by_level": {},
        "by_ip": {},
        "errors": []
    }
    
    for line in log_lines:
        entry = parse_log_line(line)
        
        # Count by level
        level = entry["level"]
        stats["by_level"][level] = stats["by_level"].get(level, 0) + 1
        
        # Count by IP
        ip = entry["ip"]
        stats["by_ip"][ip] = stats["by_ip"].get(ip, 0) + 1
        
        # Collect errors
        if level == "ERROR":
            stats["errors"].append(entry)
    
    return stats

# Sample logs
logs = [
    "2024-01-15 10:30:00 - INFO  - 192.0.2.10 - User login",
    "2024-01-15 10:31:00 - ERROR - 192.0.2.50 - Failed login",
    "2024-01-15 10:32:00 - INFO  - 192.0.2.11 - Page view",
    "2024-01-15 10:33:00 - ERROR - 192.0.2.50 - Failed login",
    "2024-01-15 10:34:00 - WARN  - 192.0.2.52 - Slow query",
    "2024-01-15 10:35:00 - ERROR - 192.0.2.50 - Failed login"
]

stats = analyze_logs(logs)

print(f"Total log entries: {stats['total']}")
print(f"\nBy Level:")
for level, count in stats["by_level"].items():
    print(f"  {level}: {count}")

print(f"\nTop IPs:")
sorted_ips = sorted(stats["by_ip"].items(), key=lambda x: x[1], reverse=True)
for ip, count in sorted_ips[:3]:
    print(f"  {ip}: {count} events")

print(f"\nErrors: {len(stats['errors'])}")
```

---

## Security-Focused Exercises

### Exercise: Subdomain Enumerator

**Problem:** Generate possible subdomains for testing.

```python
def generate_subdomains(domain, wordlist):
    """Generate list of subdomains"""
    subdomains = []
    
    for word in wordlist:
        subdomain = f"{word}.{domain}"
        subdomains.append(subdomain)
    
    return subdomains

# Common subdomain prefixes
wordlist = [
    "www", "mail", "ftp", "admin", "test",
    "dev", "staging", "api", "app", "portal",
    "secure", "vpn", "remote", "blog", "shop"
]

domain = "example.com"
subdomains = generate_subdomains(domain, wordlist)

print(f"Generated {len(subdomains)} subdomains for {domain}:")
for sub in subdomains:
    print(f"  - {sub}")
```

---

### Exercise: IP Range Generator

**Problem:** Generate all IPs in a range for network scanning.

```python
def generate_ip_range(base_ip, start, end):
    """Generate IP addresses in range"""
    ips = []
    
    for i in range(start, end + 1):
        ip = f"{base_ip}.{i}"
        ips.append(ip)
    
    return ips

# Generate 192.0.2.1 - 192.0.2.254
base = "192.0.2"
ip_range = generate_ip_range(base, 1, 254)

print(f"Generated {len(ip_range)} IP addresses")
print(f"First 5: {ip_range[:5]}")
print(f"Last 5: {ip_range[-5:]}")
```

---

### Exercise: Base64 Encoder/Decoder

**Problem:** Encode and decode strings using Base64.

```python
import base64

def encode_base64(text):
    """Encode string to base64"""
    bytes_data = text.encode('utf-8')
    encoded = base64.b64encode(bytes_data)
    return encoded.decode('utf-8')

def decode_base64(encoded_text):
    """Decode base64 to string"""
    bytes_data = encoded_text.encode('utf-8')
    decoded = base64.b64decode(bytes_data)
    return decoded.decode('utf-8')

# Test
original = "SecretMessage123!"
encoded = encode_base64(original)
decoded = decode_base64(encoded)

print(f"Original:  {original}")
print(f"Encoded:   {encoded}")
print(f"Decoded:   {decoded}")
print(f"Match:     {original == decoded}")
```

---

## Solutions & Tips

### Debugging Tips

```python
# Print variables to debug
def debug_example(numbers):
    print(f"Input: {numbers}")  # See what you're working with
    
    total = 0
    for num in numbers:
        print(f"Adding {num}, total is now {total}")  # Track progress
        total += num
    
    print(f"Final total: {total}")
    return total

debug_example([1, 2, 3, 4, 5])
```

### Common Mistakes

```python
# ❌ Modifying list while iterating
numbers = [1, 2, 3, 4, 5]
for num in numbers:
    if num % 2 == 0:
        numbers.remove(num)  # DON'T DO THIS!

# ✅ Create new list instead
numbers = [1, 2, 3, 4, 5]
odd_numbers = [num for num in numbers if num % 2 != 0]

# ❌ Infinite loop
while True:
    print("This will run forever!")  # Add break condition!

# ✅ With break condition
count = 0
while True:
    print(count)
    count += 1
    if count >= 10:
        break
```

---

## Practice Resources

### Online Platforms
- **LeetCode**: Coding challenges
- **HackerRank**: Python exercises
- **Codewars**: Gamified challenges
- **Project Euler**: Math-focused problems

### Next Steps
1. Complete all beginner exercises
2. Move to intermediate challenges
3. Try advanced projects
4. Build your own security tools

**Remember:** The best way to learn programming is by writing code. Start simple and build up!

---

## See Also

- [Functions](functions.md) - Organize your code
- [Iteration](iteration.md) - Master loops
- [Python & APIs](python-and-apis.md) - Real-world applications
