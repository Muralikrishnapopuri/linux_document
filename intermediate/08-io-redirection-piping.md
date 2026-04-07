# 🔀 I/O Redirection & Piping

Control where command input comes from and output goes to.

---

## 1. Standard Streams

Every command has three standard streams:

| Stream | Number | Default | Description |
|--------|--------|---------|------------|
| `stdin` | 0 | Keyboard | Standard input |
| `stdout` | 1 | Screen | Standard output |
| `stderr` | 2 | Screen | Standard error |

---

## 2. Output Redirection (`>`, `>>`)

### Redirect stdout to a File

```bash
# Write output to file (overwrites if exists)
echo "Hello World" > output.txt
ls -la > filelist.txt

# Append output to file
echo "Another line" >> output.txt
date >> log.txt
```

### Redirect stderr

```bash
# Redirect errors to a file
find / -name "secret" 2> errors.txt

# Redirect errors to /dev/null (discard)
find / -name "file" 2>/dev/null
```

### Redirect Both stdout and stderr

```bash
# Both to same file
command > output.txt 2>&1

# Modern syntax (bash 4+)
command &> output.txt

# Append both to file
command >> output.txt 2>&1
command &>> output.txt

# stdout to one file, stderr to another
command > output.txt 2> errors.txt
```

---

## 3. Input Redirection (`<`)

```bash
# Read input from a file
sort < unsorted.txt
wc -l < data.txt
mysql database < script.sql
```

### Here Document (`<<`)

```bash
cat << EOF
Hello, this is a
multi-line
text block.
EOF

# Write multi-line content to a file
cat << EOF > config.txt
host=localhost
port=3000
debug=true
EOF
```

### Here String (`<<<`)

```bash
grep "hello" <<< "hello world"
wc -w <<< "count these words"
```

---

## 4. Piping (`|`)

Send the output of one command as input to another.

### Basic Piping

```bash
# Filter output
ls -la | grep ".txt"

# Count files
ls | wc -l

# Sort and remove duplicates
cat file.txt | sort | uniq

# Find top 10 largest files
du -sh * | sort -rh | head -10

# Search in processes
ps aux | grep nginx
```

### Piping Chains (Multi-Step)

```bash
# Find top 5 most common words in a file
cat file.txt | tr ' ' '\n' | sort | uniq -c | sort -rn | head -5

# Find users with bash shell
cat /etc/passwd | grep "/bin/bash" | cut -d: -f1

# Monitor specific log entries
tail -f /var/log/syslog | grep --line-buffered "error"

# Count unique IP addresses in access log
cat access.log | awk '{print $1}' | sort | uniq -c | sort -rn | head -10
```

---

## 5. `tee` — Write to File AND Screen

```bash
# Display output and save to file simultaneously
ls -la | tee filelist.txt

# Append instead of overwrite
echo "new data" | tee -a log.txt

# Write to multiple files
echo "data" | tee file1.txt file2.txt

# Use in a pipeline
ls -la | tee listing.txt | grep ".sh"
```

---

## 6. `xargs` — Build Command Lines from stdin

Converts piped input into arguments for another command.

```bash
# Delete all .tmp files found by find
find . -name "*.tmp" | xargs rm

# With confirmation
find . -name "*.tmp" | xargs -p rm

# Handle filenames with spaces
find . -name "*.log" -print0 | xargs -0 rm

# Limit arguments per command
echo "1 2 3 4 5" | xargs -n 2 echo

# Parallel execution
find . -name "*.jpg" | xargs -P 4 -I {} convert {} {}.png

# Run command for each line
cat urls.txt | xargs -I {} curl -s {}
```

### xargs Options

| Option | Description |
|--------|------------|
| `-n N` | Use N arguments per command |
| `-I {}` | Replace `{}` with each input item |
| `-P N` | Run N processes in parallel |
| `-0` | Handle null-separated input |
| `-p` | Prompt before executing |
| `-t` | Print command before executing |

---

## 7. Process Substitution (`<()`)

Treat command output as a file.

```bash
# Compare output of two commands
diff <(ls dir1) <(ls dir2)

# Process output as file input
while read line; do echo "$line"; done < <(cat file.txt)
```

---

## 8. Named Pipes (FIFO)

```bash
# Create a named pipe
mkfifo mypipe

# Write to pipe (in one terminal)
echo "data" > mypipe

# Read from pipe (in another terminal)
cat < mypipe
```

---

## 9. `/dev/null` — The Black Hole

Discards any data written to it.

```bash
# Discard stdout
command > /dev/null

# Discard stderr
command 2>/dev/null

# Discard everything
command &>/dev/null

# Common use: check if command succeeded
if grep -q "pattern" file.txt 2>/dev/null; then
    echo "Found"
fi
```

---

## 10. Common Patterns

```bash
# Run command only if previous succeeded
command1 && command2

# Run command only if previous failed
command1 || command2

# Run both regardless
command1 ; command2

# Command substitution
echo "Today is $(date)"
files=$(ls *.txt)

# Subshell
(cd /tmp && ls)       # cd only affects the subshell
```

---

## 💡 Tips

- `>` overwrites; `>>` appends — always double-check
- Use `2>/dev/null` to suppress error messages
- `|` is the most powerful feature in Linux — master it
- `tee` is perfect for saving output while still seeing it
- Never redirect output to the same file you're reading: `cat file > file` will erase it!

---

[← Previous: Archiving](07-archiving-compression.md) | [Back to Index](../README.md)
