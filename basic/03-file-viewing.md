# 👁️ File Viewing & Reading

Commands to view, read, and inspect file contents.

---

## 1. `cat` — Concatenate and Display Files

### Basic Usage

```bash
cat file.txt                   # Display file contents
cat file1.txt file2.txt        # Display multiple files
cat -n file.txt                # Display with line numbers
```

### Common Options

| Option | Description | Example |
|--------|------------|---------|
| `-n` | Number all lines | `cat -n file.txt` |
| `-b` | Number non-blank lines only | `cat -b file.txt` |
| `-s` | Squeeze multiple blank lines into one | `cat -s file.txt` |
| `-E` | Show `$` at end of each line | `cat -E file.txt` |
| `-T` | Show tabs as `^I` | `cat -T file.txt` |

### Create & Append with cat

```bash
# Create a new file (type content, press Ctrl+D to save)
cat > newfile.txt

# Append to an existing file
cat >> existingfile.txt

# Combine files into one
cat file1.txt file2.txt > combined.txt
```

---

## 2. `less` — View Files Page by Page

Best for viewing large files. Allows scrolling forward and backward.

```bash
less largefile.log
```

### Navigation Inside `less`

| Key | Action |
|-----|--------|
| `Space` / `f` | Next page |
| `b` | Previous page |
| `↑` / `↓` | Scroll one line |
| `g` | Go to beginning |
| `G` | Go to end |
| `/pattern` | Search forward |
| `?pattern` | Search backward |
| `n` | Next search result |
| `N` | Previous search result |
| `q` | Quit |
| `-N` | Toggle line numbers |

---

## 3. `more` — Simple File Viewer

Simpler than `less`, only scrolls forward.

```bash
more file.txt
```

| Key | Action |
|-----|--------|
| `Space` | Next page |
| `Enter` | Next line |
| `q` | Quit |

---

## 4. `head` — View First Lines of a File

```bash
head file.txt              # Show first 10 lines (default)
head -n 20 file.txt        # Show first 20 lines
head -n 5 file.txt         # Show first 5 lines
head -c 100 file.txt       # Show first 100 bytes
```

### View first lines of multiple files

```bash
head -n 5 *.txt
```

---

## 5. `tail` — View Last Lines of a File

```bash
tail file.txt              # Show last 10 lines (default)
tail -n 20 file.txt        # Show last 20 lines
tail -n 5 file.txt         # Show last 5 lines
```

### Follow a File in Real-Time (Live Logs)

```bash
tail -f /var/log/syslog          # Follow log in real-time
tail -f -n 50 /var/log/syslog   # Show last 50 lines then follow
tail -F /var/log/syslog          # Follow even if file is rotated
```

> 💡 `tail -f` is extremely useful for monitoring log files in production.

---

## 6. `wc` — Word Count

```bash
wc file.txt                # Lines, words, characters
wc -l file.txt             # Count lines only
wc -w file.txt             # Count words only
wc -c file.txt             # Count bytes only
wc -m file.txt             # Count characters
wc -L file.txt             # Length of longest line
```

### Count across multiple files

```bash
wc -l *.txt                # Line count for each .txt file
```

---

## 7. `nl` — Number Lines

```bash
nl file.txt                # Number non-empty lines
nl -ba file.txt            # Number all lines (including blank)
nl -s '. ' file.txt        # Custom separator after number
```

---

## 8. `tac` — Reverse cat (Display File in Reverse)

```bash
tac file.txt               # Print file with last line first
```

---

## 9. `rev` — Reverse Each Line

```bash
echo "hello" | rev         # Output: olleh
rev file.txt               # Reverse every line in a file
```

---

## 10. `strings` — Extract Readable Text from Binary Files

```bash
strings /bin/ls            # Show readable strings in a binary
strings -n 10 binary.exe   # Only strings with 10+ characters
```

---

## 11. `diff` — Compare Two Files

```bash
diff file1.txt file2.txt              # Show differences
diff -u file1.txt file2.txt           # Unified format (like git)
diff -y file1.txt file2.txt           # Side by side comparison
diff -r dir1/ dir2/                   # Compare directories
diff --color file1.txt file2.txt      # Colorized output
```

---

## 12. `cmp` — Compare Two Files Byte by Byte

```bash
cmp file1.txt file2.txt    # Reports first difference
cmp -l file1.txt file2.txt # List all differing bytes
```

---

## 💡 Tips

- Use `less` instead of `cat` for large files to avoid flooding the terminal
- `tail -f` is your best friend for monitoring logs
- Combine `head` and `tail` to view a range: `head -n 20 file | tail -n 5` (lines 16-20)
- Use `wc -l` to quickly count lines in files

---

[← Previous: File Operations](02-file-operations.md) | [Back to Index](../README.md) | [Next: File Permissions →](04-file-permissions.md)
