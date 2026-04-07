# 🔍 Search & Find

Locate files, directories, and text patterns across the system.

---

## 1. `find` — Search for Files & Directories

The most powerful file search tool in Linux.

### Basic Syntax

```bash
find [path] [options] [expression]
```

### Search by Name

```bash
find . -name "file.txt"              # Exact name (case-sensitive)
find . -iname "file.txt"             # Case-insensitive
find /home -name "*.pdf"             # All PDFs in /home
find . -name "*.log" -o -name "*.txt" # .log OR .txt files
```

### Search by Type

```bash
find . -type f                       # Files only
find . -type d                       # Directories only
find . -type l                       # Symbolic links only
```

### Search by Size

```bash
find . -size +100M                   # Files larger than 100MB
find . -size -1k                     # Files smaller than 1KB
find . -size 50M                     # Files exactly 50MB
find /var -size +1G                  # Files larger than 1GB
```

| Size Suffix | Meaning |
|-------------|---------|
| `c` | Bytes |
| `k` | Kilobytes |
| `M` | Megabytes |
| `G` | Gigabytes |

### Search by Time

```bash
find . -mtime -7                     # Modified in last 7 days
find . -mtime +30                    # Modified more than 30 days ago
find . -mmin -60                     # Modified in last 60 minutes
find . -atime -1                     # Accessed in last 24 hours
find . -newer reference.txt          # Newer than reference file
```

### Search by Permissions

```bash
find . -perm 755                     # Exact permission match
find . -perm -u+x                    # User has execute
find . -perm /o+w                    # Others have write
```

### Search by Owner

```bash
find /home -user alice               # Files owned by alice
find /tmp -group developers          # Files owned by group
find / -nouser                       # Files with no owner
```

### Combine with Actions

```bash
# Delete found files
find /tmp -name "*.tmp" -delete

# Execute a command on each result
find . -name "*.log" -exec rm {} \;

# Execute with confirmation
find . -name "*.bak" -ok rm {} \;

# Print full details
find . -name "*.sh" -exec ls -la {} \;

# Count files found
find . -type f | wc -l
```

### Practical Examples

```bash
# Find large files (> 500MB)
find / -type f -size +500M 2>/dev/null

# Find empty files and directories
find . -empty

# Find and compress old logs
find /var/log -name "*.log" -mtime +30 -exec gzip {} \;

# Find files with specific extension, max 2 levels deep
find . -maxdepth 2 -name "*.py"

# Exclude a directory from search
find . -path ./node_modules -prune -o -name "*.js" -print
```

---

## 2. `locate` — Fast File Search (Uses Database)

Much faster than `find` but uses a pre-built database.

```bash
# Install if not present
sudo apt install mlocate

# Update the database (run periodically or after adding files)
sudo updatedb

# Search for files
locate file.txt
locate -i "readme"              # Case-insensitive
locate -c "*.pdf"               # Count matches
locate -l 10 "*.conf"           # Limit to 10 results
```

---

## 3. `grep` — Search Text Inside Files

### Basic Usage

```bash
grep "pattern" file.txt                # Search in a single file
grep "pattern" *.txt                   # Search in multiple files
grep -r "pattern" /path/to/dir/        # Recursive search
```

### Common Options

| Option | Description | Example |
|--------|------------|---------|
| `-i` | Case-insensitive | `grep -i "error" log.txt` |
| `-r` | Recursive (search in directories) | `grep -r "TODO" src/` |
| `-n` | Show line numbers | `grep -n "error" file.txt` |
| `-c` | Count matching lines | `grep -c "error" log.txt` |
| `-l` | Show only filenames with matches | `grep -rl "password" /etc/` |
| `-v` | Invert match (non-matching lines) | `grep -v "debug" log.txt` |
| `-w` | Match whole words only | `grep -w "error" log.txt` |
| `-A n` | Show n lines after match | `grep -A 3 "error" log.txt` |
| `-B n` | Show n lines before match | `grep -B 3 "error" log.txt` |
| `-C n` | Show n lines before and after | `grep -C 2 "error" log.txt` |
| `-E` | Extended regex (same as `egrep`) | `grep -E "err|warn" log.txt` |
| `--color` | Highlight matches | `grep --color "pattern" file` |

### Practical Examples

```bash
# Find errors in logs
grep -in "error" /var/log/syslog

# Search for a function in source code
grep -rn "def main" *.py

# Find lines NOT containing a pattern
grep -v "^#" config.conf       # Skip comment lines

# Search with regex
grep -E "^[0-9]{3}-[0-9]{4}$" phones.txt

# Search multiple patterns
grep -E "error|warning|critical" log.txt

# Count occurrences of a word
grep -o "error" log.txt | wc -l
```

---

## 4. `which` — Find Command Location

```bash
which python3            # /usr/bin/python3
which ls                 # /usr/bin/ls
which node               # Shows path or nothing
```

---

## 5. `whereis` — Find Binary, Source, and Manual Pages

```bash
whereis python3          # binary, source, and man pages
whereis ls
whereis gcc
```

---

## 6. `type` — Describe a Command

```bash
type ls                  # ls is aliased to 'ls --color=auto'
type cd                  # cd is a shell builtin
type python3             # python3 is /usr/bin/python3
```

---

## 7. `whatis` — One-Line Command Description

```bash
whatis ls                # ls (1) - list directory contents
whatis grep              # grep (1) - print lines matching a pattern
```

---

## 💡 Tips

- Use `find` for precise, real-time searches; use `locate` for speed
- Always run `sudo updatedb` before using `locate` for up-to-date results
- `grep -rn` is the most common combo for searching code
- Pipe `find` results into `xargs` for efficient batch operations: `find . -name "*.tmp" | xargs rm`

---

[← Previous: Permissions](04-file-permissions.md) | [Back to Index](../README.md) | [Next: System Info →](06-system-info.md)
