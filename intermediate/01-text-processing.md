# ✂️ Text Processing

Transform, filter, and manipulate text data from the command line.

---

## 1. `sed` — Stream Editor

Process and transform text line by line.

### Substitution (Find & Replace)

```bash
# Replace first occurrence per line
sed 's/old/new/' file.txt

# Replace ALL occurrences per line
sed 's/old/new/g' file.txt

# Case-insensitive replace
sed 's/old/new/gI' file.txt

# Edit file in-place
sed -i 's/old/new/g' file.txt

# Edit in-place with backup
sed -i.bak 's/old/new/g' file.txt
```

### Line Operations

```bash
# Delete specific line (line 5)
sed '5d' file.txt

# Delete range of lines (lines 3-7)
sed '3,7d' file.txt

# Delete lines matching pattern
sed '/pattern/d' file.txt

# Delete blank lines
sed '/^$/d' file.txt

# Print only line 10
sed -n '10p' file.txt

# Print lines 5-10
sed -n '5,10p' file.txt

# Insert text before line 3
sed '3i\New line of text' file.txt

# Append text after line 3
sed '3a\New line of text' file.txt
```

### Practical Examples

```bash
# Remove HTML tags
sed 's/<[^>]*>//g' page.html

# Add line numbers
sed = file.txt | sed 'N; s/\n/\t/'

# Replace multiple patterns
sed -e 's/foo/bar/g' -e 's/baz/qux/g' file.txt

# Comment out a line containing "debug"
sed '/debug/s/^/# /' config.txt
```

---

## 2. `awk` — Pattern Scanning & Processing

Powerful text processing with field-based operations.

### Basic Syntax

```bash
awk '{action}' file.txt
awk '/pattern/ {action}' file.txt
```

### Field Extraction

By default, fields are separated by whitespace. `$1` is field 1, `$2` is field 2, `$0` is the entire line.

```bash
# Print first column
awk '{print $1}' file.txt

# Print first and third columns
awk '{print $1, $3}' file.txt

# Print last column
awk '{print $NF}' file.txt

# Custom delimiter (e.g., colon)
awk -F: '{print $1, $3}' /etc/passwd

# CSV files (comma delimiter)
awk -F, '{print $1, $2}' data.csv
```

### Filtering

```bash
# Print lines matching pattern
awk '/error/' log.txt

# Print lines where field 3 > 100
awk '$3 > 100' data.txt

# Print lines where field 1 equals "admin"
awk '$1 == "admin"' users.txt

# Print line numbers
awk '{print NR, $0}' file.txt
```

### Built-in Variables

| Variable | Meaning |
|----------|---------|
| `$0` | Entire line |
| `$1, $2...` | Field 1, 2, etc. |
| `NR` | Current line number |
| `NF` | Number of fields in current line |
| `FS` | Field separator |
| `OFS` | Output field separator |

### Practical Examples

```bash
# Sum values in column 2
awk '{sum += $2} END {print sum}' data.txt

# Average of column 3
awk '{sum += $3; count++} END {print sum/count}' data.txt

# Print users from /etc/passwd
awk -F: '{print $1}' /etc/passwd

# Print disk usage over 80%
df -h | awk '$5+0 > 80 {print $6, $5}'
```

---

## 3. `cut` — Extract Sections from Lines

```bash
# Cut by character position
cut -c1-5 file.txt               # Characters 1-5
cut -c1,3,5 file.txt             # Characters 1, 3, 5

# Cut by delimiter and field
cut -d',' -f1 data.csv           # First field (comma-separated)
cut -d':' -f1,3 /etc/passwd     # Fields 1 and 3 (colon-separated)
cut -d' ' -f2- file.txt         # Field 2 onwards

# Cut by bytes
cut -b1-10 file.txt
```

---

## 4. `sort` — Sort Lines

```bash
sort file.txt                    # Alphabetical sort
sort -n file.txt                 # Numeric sort
sort -r file.txt                 # Reverse sort
sort -k2 file.txt                # Sort by 2nd column
sort -k2 -n file.txt             # Sort by 2nd column numerically
sort -t',' -k3 data.csv         # Sort CSV by 3rd column
sort -u file.txt                 # Sort and remove duplicates
sort -h file.txt                 # Human-readable numbers (1K, 2M)
```

---

## 5. `uniq` — Report or Remove Duplicate Lines

> **Note:** Input must be sorted first for `uniq` to work correctly.

```bash
sort file.txt | uniq             # Remove duplicates
sort file.txt | uniq -c          # Count occurrences
sort file.txt | uniq -d          # Show only duplicates
sort file.txt | uniq -u          # Show only unique lines
sort file.txt | uniq -ci         # Count, case-insensitive
```

---

## 6. `tr` — Translate or Delete Characters

```bash
# Convert lowercase to uppercase
echo "hello" | tr 'a-z' 'A-Z'

# Convert uppercase to lowercase
echo "HELLO" | tr 'A-Z' 'a-z'

# Replace spaces with underscores
echo "hello world" | tr ' ' '_'

# Delete specific characters
echo "hello 123" | tr -d '0-9'

# Squeeze repeated characters
echo "hello     world" | tr -s ' '

# Delete newlines
cat file.txt | tr -d '\n'

# Replace non-alphanumeric with newline (word per line)
tr -cs 'A-Za-z0-9' '\n' < file.txt
```

---

## 7. `paste` — Merge Lines from Files

```bash
# Merge two files side by side
paste file1.txt file2.txt

# Custom delimiter
paste -d',' file1.txt file2.txt

# Join all lines into one (comma-separated)
paste -s -d',' file.txt
```

---

## 8. `column` — Format Output into Columns

```bash
# Format as table
cat data.txt | column -t

# Specify delimiter
cat data.csv | column -t -s ','
```

---

## 9. `tee` — Read from stdin and Write to File + stdout

```bash
echo "hello" | tee output.txt           # Write to file and display
echo "hello" | tee -a output.txt        # Append instead of overwrite
ls -la | tee filelist.txt               # Save ls output to file
```

---

## 💡 Tips

- Combine tools with pipes: `cat file | sort | uniq -c | sort -rn | head`
- `awk` can do everything `cut` can, and more
- Always `sort` before `uniq`
- `sed -i` modifies files in-place — use `-i.bak` to keep a backup

---

[Back to Index](../README.md) | [Next: Process Management →](02-process-management.md)
