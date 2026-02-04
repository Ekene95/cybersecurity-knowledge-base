# Bash Scripting Examples

Practical bash scripting examples for security testing and system administration.

## IP Geolocation Script

Download IPs file and analyze which IPs belong to specific countries (China and Netherlands).

### Method 1: Using `whois`

```bash
#!/bin/bash
echo "Provide an IPs file:"
read file
for i in $(cat $file)
do
    country=$(whois $i | grep -i country | head -1 | awk '{print $2}')
    if [ $country == "CN" ] || [ $country == "NL" ]
    then
        echo $i $country
    fi
done
```

### Method 2: Using `geoiplookup`

```bash
#!/bin/bash
echo "Provide an IPs file:"
read file
for i in $(cat $file)
do
    country=$(geoiplookup $i | awk '{print $2}')
    if [ $country == "CN" ] || [ $country == "NL" ]
    then
        echo $i $country
    fi
done
```

**Setup for geoiplookup:**
```bash
sudo apt-get install geoip-bin
```

**Note:** `geoiplookup` is generally faster and more reliable than parsing `whois` output.

## Batch File Download

Download numbered files from a web server:

```bash
# Download files 1 through 100
for i in $(seq 1 100); do 
    wget 3.72.47.184/$i
done
```

### Enhanced Version with Error Handling

```bash
#!/bin/bash
BASE_URL="3.72.47.184"
START=1
END=100

for i in $(seq $START $END); do
    echo "Downloading file $i..."
    wget -q "$BASE_URL/$i" -O "file_$i"
    if [ $? -eq 0 ]; then
        echo "✓ Downloaded file $i"
    else
        echo "✗ Failed to download file $i"
    fi
done
```

## Script Best Practices

### Use Variables for Configuration

```bash
#!/bin/bash

# Configuration
INPUT_FILE="ips.txt"
OUTPUT_FILE="results.txt"
COUNTRIES=("CN" "NL" "RU")

# Validation
if [ ! -f "$INPUT_FILE" ]; then
    echo "Error: $INPUT_FILE not found"
    exit 1
fi

# Processing
while read ip; do
    country=$(geoiplookup $ip | awk '{print $2}')
    for target_country in "${COUNTRIES[@]}"; do
        if [ "$country" == "$target_country" ]; then
            echo "$ip $country" >> "$OUTPUT_FILE"
            break
        fi
    done
done < "$INPUT_FILE"
```

### Error Handling

```bash
#!/bin/bash
set -e  # Exit on error
set -u  # Exit on undefined variable
set -o pipefail  # Exit on pipe failure

# Function for error handling
error_exit() {
    echo "Error: $1" >&2
    exit 1
}

# Usage
[ -f "$INPUT_FILE" ] || error_exit "Input file not found"
```

### Logging

```bash
#!/bin/bash

LOG_FILE="/var/log/script.log"

log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1" | tee -a "$LOG_FILE"
}

log "Script started"
# Your code here
log "Script completed"
```

## Useful Script Templates

### Network Scanner

```bash
#!/bin/bash
# Scan a network range

NETWORK="192.168.1"
START=1
END=254

for i in $(seq $START $END); do
    IP="$NETWORK.$i"
    ping -c 1 -W 1 $IP > /dev/null 2>&1
    if [ $? -eq 0 ]; then
        echo "$IP is alive"
    fi
done
```

### Log Analyzer

```bash
#!/bin/bash
# Analyze auth.log for suspicious activity

LOG_FILE="/var/log/auth.log"
THRESHOLD=10

echo "=== Failed Login Analysis ==="
echo "IPs with more than $THRESHOLD failed attempts:"
grep "Failed password" "$LOG_FILE" | \
    awk '{print $(NF-3)}' | \
    sort | uniq -c | sort -nr | \
    awk -v threshold=$THRESHOLD '$1 > threshold {print $1, $2}'
```

### Batch File Processor

```bash
#!/bin/bash
# Process all files in a directory

DIRECTORY="/path/to/files"
EXTENSION="*.log"

for file in $DIRECTORY/$EXTENSION; do
    if [ -f "$file" ]; then
        echo "Processing: $(basename $file)"
        # Your processing commands here
        # Example: cat "$file" | grep "ERROR" > "$file.errors"
    fi
done
```

### System Health Check

```bash
#!/bin/bash
# Quick system health check

echo "=== System Health Check ==="
echo "Date: $(date)"
echo ""

echo "=== Disk Usage ==="
df -h | grep -v tmpfs

echo ""
echo "=== Memory Usage ==="
free -h

echo ""
echo "=== Load Average ==="
uptime

echo ""
echo "=== Top 5 CPU Processes ==="
ps aux --sort=-%cpu | head -6

echo ""
echo "=== Top 5 Memory Processes ==="
ps aux --sort=-%mem | head -6
```

## Command Line Arguments

### Basic Argument Parsing

```bash
#!/bin/bash

# Check arguments
if [ $# -lt 1 ]; then
    echo "Usage: $0 <filename>"
    exit 1
fi

FILENAME=$1
echo "Processing: $FILENAME"
```

### Advanced Argument Parsing

```bash
#!/bin/bash

# Default values
VERBOSE=false
OUTPUT=""

# Parse arguments
while [[ $# -gt 0 ]]; do
    case $1 in
        -v|--verbose)
            VERBOSE=true
            shift
            ;;
        -o|--output)
            OUTPUT="$2"
            shift 2
            ;;
        *)
            echo "Unknown option: $1"
            exit 1
            ;;
    esac
done

# Use variables
if [ "$VERBOSE" = true ]; then
    echo "Verbose mode enabled"
fi
```

## Debugging Tips

### Enable Debug Mode

```bash
#!/bin/bash
set -x  # Print commands as they execute

# Your code here

set +x  # Disable debug mode
```

### Conditional Debugging

```bash
#!/bin/bash

DEBUG=${DEBUG:-false}

debug() {
    if [ "$DEBUG" = true ]; then
        echo "DEBUG: $1" >&2
    fi
}

debug "Starting script"
# Your code here
debug "Script completed"
```

Run with: `DEBUG=true ./script.sh`

## See Also

- [File Manipulation](file-manipulation.md) - Text processing in scripts
- [Finding Files](finding-files.md) - File search operations
- [Services](services.md) - Service management in scripts
