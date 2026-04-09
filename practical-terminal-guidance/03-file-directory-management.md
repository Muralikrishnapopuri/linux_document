# 📁 File & Directory Management Recipes

Practical recipes for everyday file and directory tasks.

---

## 1. Creating Project Structures

### Quick Project Setup

```bash
# Create a full project structure in one command
mkdir -p myproject/{src/{components,utils,assets},docs,tests,config}

# Create project with files
mkdir -p myapp/{src,tests,docs} && \
  touch myapp/{README.md,Makefile,.gitignore} && \
  touch myapp/src/{main.py,config.py,utils.py} && \
  touch myapp/tests/{test_main.py,test_utils.py}

# View the created structure
tree myapp
```

**Output:**
```
myapp/
├── .gitignore
├── Makefile
├── README.md
├── docs/
├── src/
│   ├── config.py
│   ├── main.py
│   └── utils.py
└── tests/
    ├── test_main.py
    └── test_utils.py
```

---

## 2. Bulk Rename Files

### Using `rename` (Perl-based)

```bash
# Install rename utility
sudo apt install rename

# Change all .txt extensions to .md
rename 's/\.txt$/.md/' *.txt

# Add a prefix to all files
rename 's/^/2024_/' *.jpg

# Remove spaces from filenames (replace with underscore)
rename 's/ /_/g' *

# Convert filenames to lowercase
rename 'y/A-Z/a-z/' *

# Add sequential numbers to files
n=1; for f in *.jpg; do mv "$f" "$(printf 'photo_%03d.jpg' $n)"; ((n++)); done
```

### Using Parameter Expansion

```bash
# Rename file extension (.txt → .bak)
for f in *.txt; do mv "$f" "${f%.txt}.bak"; done

# Remove a prefix from all files
for f in old_*; do mv "$f" "${f#old_}"; done

# Replace a substring in filenames
for f in *report*; do mv "$f" "${f//report/summary}"; done
```

---

## 3. Finding Files Efficiently

### Common `find` Recipes

```bash
# Find files modified in the last 24 hours
find . -type f -mtime -1

# Find files larger than 100MB
find / -type f -size +100M 2>/dev/null

# Find and delete empty files
find . -type f -empty -delete

# Find files by extension, excluding a directory
find . -name "*.log" -not -path "./archive/*"

# Find files owned by a specific user
find /home -user murali-krishna -type f

# Find files with specific permissions
find . -perm 777 -type f

# Find recently accessed files (last 30 minutes)
find . -type f -amin -30

# Find and list with details
find /var/log -name "*.log" -exec ls -lh {} \;
```

### Using `locate` (Faster but Uses Database)

```bash
# Install and update the database
sudo apt install mlocate
sudo updatedb

# Search for files
locate nginx.conf

# Case-insensitive search
locate -i readme

# Count matches
locate -c "*.py"
```

---

## 4. File Comparison

```bash
# Compare two files line by line
diff file1.txt file2.txt

# Side-by-side comparison
diff -y file1.txt file2.txt

# Colorized diff
diff --color file1.txt file2.txt

# Show only lines that differ
diff --brief file1.txt file2.txt

# Compare directories
diff -rq dir1/ dir2/

# Three-way merge
diff3 mine.txt original.txt theirs.txt
```

---

## 5. Disk Usage Analysis

```bash
# Check overall disk usage
df -h

# Check directory sizes (sorted, top 10)
du -sh */ | sort -rh | head -10

# Find total size of a directory
du -sh /var/log

# Show sizes of hidden files too
du -ah --max-depth=1 | sort -rh | head -20

# Find the biggest files on the system
sudo find / -type f -exec du -h {} + 2>/dev/null | sort -rh | head -20

# Interactive disk usage (ncurses)
sudo apt install ncdu
ncdu /
```

---

## 6. File Permissions Quick Reference

### Understanding Permission Numbers

```
rwx = 4+2+1 = 7 (read + write + execute)
rw- = 4+2+0 = 6 (read + write)
r-x = 4+0+1 = 5 (read + execute)
r-- = 4+0+0 = 4 (read only)
```

### Common Permission Patterns

```bash
# Make a script executable
chmod +x script.sh

# Standard file permissions
chmod 644 file.txt          # Owner: rw, Group: r, Others: r

# Standard directory permissions
chmod 755 /var/www/html     # Owner: rwx, Group: rx, Others: rx

# Private file (only owner can read/write)
chmod 600 ~/.ssh/id_rsa

# Recursively fix permissions
find /var/www -type d -exec chmod 755 {} \;
find /var/www -type f -exec chmod 644 {} \;

# Change ownership recursively
sudo chown -R www-data:www-data /var/www/html
```

---

## 7. Symbolic and Hard Links

```bash
# Create a symbolic link (shortcut)
ln -s /path/to/original /path/to/link

# Create a symbolic link to a directory
ln -s /var/log /home/user/logs

# Create a hard link (same file, different name)
ln original.txt hardlink.txt

# Find broken symbolic links
find . -xtype l

# Update a symbolic link
ln -sf /new/target /path/to/link

# List files showing link targets
ls -la /etc/alternatives/
```

---

## 8. File Type Identification

```bash
# Identify file type
file document.pdf
# Output: document.pdf: PDF document, version 1.7

file mystery_file
# Output: mystery_file: JPEG image data, JFIF standard 1.01

file script.sh
# Output: script.sh: Bourne-Again shell script, ASCII text executable

# Check MIME type
file --mime-type image.png
# Output: image.png: image/png

# Check file encoding
file --mime-encoding file.txt
# Output: file.txt: utf-8
```

---

## 9. Watching for File Changes

```bash
# Watch a file for changes in real-time
tail -f /var/log/syslog

# Watch multiple files
tail -f /var/log/*.log

# Use inotifywait to monitor directory changes
sudo apt install inotify-tools

# Watch for any file changes in a directory
inotifywait -m -r /path/to/dir

# Watch and execute a command on change
inotifywait -m -e modify /path/to/dir | while read event; do
  echo "File changed: $event"
done

# Watch command output refresh every N seconds
watch -n 5 'ls -la /tmp'
```

---

## 💡 Tips

- Use `mkdir -p` to create nested directories without errors
- Always use quotes around filenames with spaces: `"my file.txt"`
- Use `find ... -print0 | xargs -0` for safe handling of special filenames
- Before bulk operations, do a dry run: `rename -n 's/old/new/' *` (shows what would happen)
- `ncdu` is an excellent interactive tool for finding what's eating your disk space

---

[← Back to Index](../README.md) | [Next: Text Processing Recipes →](04-text-processing-recipes.md)
