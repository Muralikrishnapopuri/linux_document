# 🔗 Command Chaining & Workflows

Build powerful one-liners by chaining commands together.

---

## 1. Piping (`|`) — Connect Commands

Pipes send the output of one command as input to another.

```bash
# Count number of files in a directory
ls -1 | wc -l

# Find the 5 largest files in current directory
du -sh * | sort -rh | head -5

# List unique file extensions in a directory
find . -type f | sed 's/.*\.//' | sort -u

# Monitor a log file for errors
tail -f /var/log/syslog | grep --color "error"

# Show top 10 most used commands
history | awk '{print $2}' | sort | uniq -c | sort -rn | head -10
```

---

## 2. Redirection — Control Input/Output

### Output Redirection

```bash
# Write output to a file (overwrite)
echo "Hello World" > output.txt

# Append output to a file
echo "Another line" >> output.txt

# Redirect errors to a file
command_that_fails 2> errors.log

# Redirect both stdout and stderr
command 2>&1 > all_output.log

# Discard all output
command > /dev/null 2>&1
```

### Input Redirection

```bash
# Read input from a file
sort < unsorted.txt

# Here document (multi-line input)
cat << EOF > config.txt
server=localhost
port=8080
debug=true
EOF

# Here string
grep "hello" <<< "hello world"
```

---

## 3. `xargs` — Build Commands from Input

`xargs` takes input and converts it into arguments for another command.

```bash
# Delete all .tmp files found by find
find . -name "*.tmp" | xargs rm -f

# Run a command on each line of a file
cat urls.txt | xargs -I {} curl -s {}

# Process items in parallel (4 at a time)
find . -name "*.jpg" | xargs -P 4 -I {} convert {} -resize 800x600 resized_{}

# Prompt before each execution
find . -name "*.log" -mtime +30 | xargs -p rm

# Handle filenames with spaces
find . -name "*.txt" -print0 | xargs -0 wc -l
```

---

## 4. `tee` — Write to File AND Screen

```bash
# Save output to file while still displaying it
ls -la | tee file_list.txt

# Append to file instead of overwriting
echo "Log entry" | tee -a logfile.txt

# Write to multiple files simultaneously
echo "Config value" | tee file1.txt file2.txt file3.txt

# Use with sudo (common trick)
echo "new line" | sudo tee -a /etc/hosts
```

---

## 5. Process Substitution

Use the output of one command as if it were a file.

```bash
# Compare outputs of two commands
diff <(ls dir1/) <(ls dir2/)

# Compare sorted vs unsorted files
diff <(sort file1.txt) <(sort file2.txt)

# Merge two sorted files
paste <(sort file1.txt) <(sort file2.txt)

# Feed multiple inputs to a command
comm <(sort file1.txt) <(sort file2.txt)
```

---

## 6. Practical Workflow Examples

### 🔍 Log Analysis Workflow

```bash
# Find the most common IP addresses in access log
cat /var/log/apache2/access.log \
  | awk '{print $1}' \
  | sort \
  | uniq -c \
  | sort -rn \
  | head -20

# Find 404 errors with timestamps
grep " 404 " /var/log/apache2/access.log \
  | awk '{print $4, $7}' \
  | tail -20
```

### 📁 File Management Workflow

```bash
# Find and remove empty directories
find . -type d -empty -delete

# Find duplicate files by size
find . -type f -printf '%s %p\n' | sort -n | uniq -D -w 10

# Rename all .jpeg files to .jpg
for f in *.jpeg; do mv "$f" "${f%.jpeg}.jpg"; done

# Create a backup of all .conf files
find /etc -name "*.conf" -exec cp {} /backup/configs/ \;
```

### 🖥️ System Monitoring Workflow

```bash
# Check disk usage and alert if > 80%
df -h | awk '$5+0 > 80 {print "WARNING: " $6 " is " $5 " full"}'

# List all listening ports with process names
sudo ss -tlnp | awk 'NR>1 {print $4, $6}'

# Monitor memory usage every 2 seconds
watch -n 2 'free -h | head -2'

# Find processes consuming most memory
ps aux --sort=-%mem | head -10
```

### 🌐 Network Workflow

```bash
# Test connectivity to multiple servers
for server in google.com github.com 8.8.8.8; do
  ping -c 1 -W 2 "$server" > /dev/null 2>&1 && \
    echo "✅ $server is reachable" || \
    echo "❌ $server is unreachable"
done

# Download files from a URL list
cat urls.txt | xargs -P 5 -I {} wget -q {}

# Get HTTP status codes for a list of URLs
while read url; do
  code=$(curl -s -o /dev/null -w "%{http_code}" "$url")
  echo "$code $url"
done < urls.txt
```

---

## 7. Command Substitution

Embed one command's output inside another.

```bash
# Save today's date in a filename
cp file.txt "file_backup_$(date +%Y%m%d).txt"

# Count files in a directory and print message
echo "There are $(ls -1 | wc -l) files in this directory"

# Create a directory with today's date
mkdir "logs_$(date +%F)"

# Kill all processes matching a pattern
kill $(pgrep -f "python server.py")

# Set a variable from command output
MY_IP=$(hostname -I | awk '{print $1}')
echo "My IP: $MY_IP"
```

---

## 💡 Tips

- Build complex pipes incrementally — test each step before adding the next
- Use `tee` in the middle of a pipe to debug: `cmd1 | tee /tmp/debug.txt | cmd2`
- `xargs -P` enables parallel execution — great for batch processing
- Use `set -o pipefail` in scripts to catch errors in any part of a pipe
- Always quote variables in pipes to handle spaces: `"$var"` not `$var`

---

[← Back to Index](../README.md) | [Next: File & Directory Management →](03-file-directory-management.md)
