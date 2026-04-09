# 📜 Shell Scripting Basics

Write automated scripts to perform complex tasks.

---

## 1. Creating Your First Script

### Step 1: Create the File

```bash
nano myscript.sh
```

### Step 2: Add the Shebang and Content

```bash
#!/bin/bash
# My first shell script

echo "Hello, World!"
echo "Today is $(date)"
echo "You are: $(whoami)"
```

### Step 3: Make it Executable and Run

```bash
chmod +x myscript.sh
./myscript.sh
```

---

## 2. Variables

```bash
#!/bin/bash

# Declare variables (no spaces around =)
name="Alice"
age=25
current_date=$(date +%Y-%m-%d)

# Use variables with $
echo "Name: $name"
echo "Age: $age"
echo "Date: $current_date"

# Read user input
echo "Enter your name:"
read user_name
echo "Hello, $user_name!"

# Read with prompt
read -p "Enter your age: " user_age
echo "You are $user_age years old"

# Read password (hidden input)
read -sp "Enter password: " password
echo ""
```

### Special Variables

| Variable | Description |
|----------|------------|
| `$0` | Script name |
| `$1, $2...` | Positional arguments |
| `$#` | Number of arguments |
| `$@` | All arguments (as separate words) |
| `$*` | All arguments (as single string) |
| `$?` | Exit status of last command |
| `$$` | Process ID of current script |
| `$!` | PID of last background command |

```bash
#!/bin/bash
echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "Total arguments: $#"
echo "All arguments: $@"
```

---

## 3. Conditional Statements

### if / elif / else

```bash
#!/bin/bash

read -p "Enter a number: " num


if [ $num -gt 100 ]; then
    echo "Greater than 100"
elif [ $num -gt 50 ]; then
    echo "Between 51 and 100"
elif [ $num -gt 0 ]; then
    echo "Between 1 and 50"
else
    echo "Zero or negative"
fi
```

### Comparison Operators

#### Numeric Comparisons

| Operator | Meaning |
|----------|---------|
| `-eq` | Equal |
| `-ne` | Not equal |
| `-gt` | Greater than |
| `-lt` | Less than |
| `-ge` | Greater or equal |
| `-le` | Less or equal |

#### String Comparisons

| Operator | Meaning |
|----------|---------|
| `=` or `==` | Equal |
| `!=` | Not equal |
| `-z` | String is empty |
| `-n` | String is not empty |

#### File Tests

| Operator | Meaning |
|----------|---------|
| `-e` | File exists |
| `-f` | Is a regular file |
| `-d` | Is a directory |
| `-r` | Is readable |
| `-w` | Is writable |
| `-x` | Is executable |
| `-s` | File has size > 0 |

### Practical Examples

```bash
#!/bin/bash

# Check if file exists
if [ -f "/etc/passwd" ]; then
    echo "File exists"
fi

# Check if directory exists
if [ ! -d "/tmp/mydir" ]; then
    mkdir /tmp/mydir
    echo "Directory created"
fi

# String comparison
if [ "$USER" == "root" ]; then
    echo "Running as root"
else
    echo "Running as $USER"
fi

# Multiple conditions
if [ $age -gt 18 ] && [ $age -lt 65 ]; then
    echo "Working age"
fi
```

---

## 4. Loops

### for Loop

```bash
#!/bin/bash

# Loop through a list
for fruit in apple banana cherry; do
    echo "Fruit: $fruit"
done

# Loop through numbers
for i in {1..10}; do
    echo "Number: $i"
done

# C-style for loop
for ((i=0; i<5; i++)); do
    echo "Count: $i"
done

# Loop through files
for file in *.txt; do
    echo "Processing: $file"
done

# Loop through command output
for user in $(cat /etc/passwd | cut -d: -f1); do
    echo "User: $user"
done
```

### while Loop

```bash
#!/bin/bash

# Basic while loop
count=1
while [ $count -le 5 ]; do
    echo "Count: $count"
    ((count++))
done

# Read file line by line
while IFS= read -r line; do
    echo "Line: $line"
done < file.txt

# Infinite loop (until Ctrl+C)
while true; do
    echo "Running... (Ctrl+C to stop)"
    sleep 1
done
```

### until Loop

```bash
#!/bin/bash

count=1
until [ $count -gt 5 ]; do
    echo "Count: $count"
    ((count++))
done
```

---

## 5. Functions

```bash
#!/bin/bash

# Define a function
greet() {
    echo "Hello, $1!"
}

# Function with return value
add() {
    local result=$(( $1 + $2 ))
    echo $result
}

# Function with local variables
calculate() {
    local num1=$1
    local num2=$2
    local sum=$((num1 + num2))
    echo "Sum: $sum"
}

# Call functions
greet "Alice"
greet "Bob"

sum=$(add 5 3)
echo "5 + 3 = $sum"

calculate 10 20
```

---

## 6. Arrays

```bash
#!/bin/bash

# Declare an array
fruits=("apple" "banana" "cherry" "date")

# Access elements
echo "${fruits[0]}"              # First element
echo "${fruits[2]}"              # Third element
echo "${fruits[@]}"              # All elements
echo "${#fruits[@]}"             # Array length

# Loop through array
for fruit in "${fruits[@]}"; do
    echo "Fruit: $fruit"
done

# Add element
fruits+=("elderberry")

# Remove element
unset fruits[1]
```

---

## 7. String Operations

```bash
#!/bin/bash

str="Hello, World!"

echo "${#str}"                   # Length: 13
echo "${str:0:5}"                # Substring: Hello
echo "${str^^}"                  # Uppercase: HELLO, WORLD!
echo "${str,,}"                  # Lowercase: hello, world!
echo "${str/World/Linux}"        # Replace: Hello, Linux!
echo "${str%!}"                  # Remove trailing: Hello, World
```

---

## 8. Error Handling

```bash
#!/bin/bash

# Exit on error
set -e

# Exit on undefined variable
set -u

# Pipe failure
set -o pipefail

# Combined (recommended)
set -euo pipefail

# Trap errors
trap 'echo "Error on line $LINENO"; exit 1' ERR

# Check command success
if ! command -v docker &>/dev/null; then
    echo "Docker is not installed"
    exit 1
fi
```

---

## 9. Useful Script Template

```bash
#!/bin/bash
set -euo pipefail

# ============================================
# Script: backup.sh
# Description: Backup a directory
# Usage: ./backup.sh <source_dir> <dest_dir>
# ============================================

# Check arguments
if [ $# -ne 2 ]; then
    echo "Usage: $0 <source> <destination>"
    exit 1
fi

SOURCE="$1"
DEST="$2"
DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_NAME="backup_${DATE}.tar.gz"

# Check if source exists
if [ ! -d "$SOURCE" ]; then
    echo "Error: Source directory '$SOURCE' not found"
    exit 1
fi

# Create backup
echo "Creating backup of $SOURCE..."
tar -czf "${DEST}/${BACKUP_NAME}" "$SOURCE"

echo "Backup created: ${DEST}/${BACKUP_NAME}"
echo "Size: $(du -h "${DEST}/${BACKUP_NAME}" | cut -f1)"
```

---

## 💡 Tips

- Always start scripts with `#!/bin/bash` (shebang)
- Use `set -euo pipefail` for safer scripts
- Quote variables: `"$var"` to prevent word splitting
- Use `shellcheck` to lint your scripts: `sudo apt install shellcheck`
- Test scripts with `bash -x script.sh` for debug output

---

[Back to Index](../README.md) | [Next: Systemd & Services →](02-systemd-services.md)
