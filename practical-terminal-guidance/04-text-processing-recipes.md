# 📝 Text Processing Recipes

Practical examples for manipulating text data in the terminal.

---

## 1. Searching Text with `grep`

### Basic Searches

```bash
# Search for a word in a file
grep "error" logfile.txt

# Case-insensitive search
grep -i "warning" logfile.txt

# Search recursively in directories
grep -r "TODO" ./src/

# Show line numbers
grep -n "function" script.js

# Show context (3 lines before and after match)
grep -C 3 "Exception" error.log

# Count occurrences
grep -c "404" access.log

# Invert match (lines that DON'T contain the pattern)
grep -v "DEBUG" app.log

# Search for multiple patterns
grep -E "error|warning|critical" logfile.txt
```

### Advanced grep Patterns

```bash
# Search for whole words only
grep -w "port" config.txt           # Matches "port" but not "export"

# Show only the matched part
grep -o '[0-9]\+\.[0-9]\+\.[0-9]\+\.[0-9]\+' access.log   # Extract IPs

# List files containing a pattern
grep -rl "password" /etc/

# Search for lines starting/ending with a pattern
grep "^#" config.txt                # Lines starting with #
grep "\.conf$" filelist.txt         # Lines ending with .conf

# Exclude directories from recursive search
grep -r "TODO" --exclude-dir={.git,node_modules} .

# Exclude file types
grep -r "error" --include="*.log" /var/log/
```

---

## 2. `sed` — Stream Editor Recipes

### Find and Replace

```bash
# Replace first occurrence on each line
sed 's/old/new/' file.txt

# Replace ALL occurrences on each line
sed 's/old/new/g' file.txt

# Replace in-place (modify the file directly)
sed -i 's/old/new/g' file.txt

# Replace with backup
sed -i.bak 's/old/new/g' file.txt

# Case-insensitive replace
sed 's/hello/world/gI' file.txt
```

### Line Operations

```bash
# Delete lines containing a pattern
sed '/DEBUG/d' logfile.txt

# Delete blank lines
sed '/^$/d' file.txt

# Delete lines 5 to 10
sed '5,10d' file.txt

# Print only lines 10 to 20
sed -n '10,20p' file.txt

# Insert text before line 3
sed '3i\New line of text' file.txt

# Append text after line 5
sed '5a\Appended line' file.txt

# Replace entire line matching a pattern
sed '/^server=/c\server=192.168.1.100' config.txt
```

### Practical `sed` Examples

```bash
# Remove HTML tags
sed 's/<[^>]*>//g' page.html

# Add line numbers
sed = file.txt | sed 'N;s/\n/\t/'

# Extract text between two patterns
sed -n '/START/,/END/p' file.txt

# Comment out all lines containing a word
sed '/database/s/^/# /' config.txt

# Replace in multiple files
find . -name "*.conf" -exec sed -i 's/old_host/new_host/g' {} \;

# Remove trailing whitespace
sed -i 's/[[:space:]]*$//' file.txt
```

---

## 3. `awk` — Data Processing Recipes

### Column Extraction

```bash
# Print specific columns
awk '{print $1, $3}' file.txt

# Print last column
awk '{print $NF}' file.txt

# Print second-to-last column  
awk '{print $(NF-1)}' file.txt

# Use custom field separator
awk -F: '{print $1, $7}' /etc/passwd       # Username and shell
awk -F, '{print $2}' data.csv              # Second column of CSV
```

### Filtering and Conditions

```bash
# Print lines where column 3 > 100
awk '$3 > 100' data.txt

# Print lines matching a pattern
awk '/error/' logfile.txt

# Print lines longer than 80 characters
awk 'length > 80' file.txt

# Print lines where a field matches a value
awk -F: '$7 == "/bin/bash"' /etc/passwd     # Users with bash shell

# Skip header line
awk 'NR > 1 {print $0}' data.csv
```

### Calculations and Aggregation

```bash
# Sum a column
awk '{sum += $1} END {print "Total:", sum}' numbers.txt

# Calculate average
awk '{sum += $1; count++} END {print "Average:", sum/count}' numbers.txt

# Find min and max
awk 'BEGIN {min=99999; max=0} {if($1<min) min=$1; if($1>max) max=$1} END {print "Min:", min, "Max:", max}' numbers.txt

# Count unique values in a column
awk '{count[$1]++} END {for (val in count) print val, count[val]}' data.txt | sort -k2 -rn
```

### Practical `awk` Examples

```bash
# Parse disk usage (show partitions > 50% full)
df -h | awk '+$5 > 50 {print $6, $5}'

# Parse /etc/passwd for user info
awk -F: '{printf "%-15s %s\n", $1, $6}' /etc/passwd

# Process CSV and calculate totals per category
awk -F, '{sales[$1] += $3} END {for (cat in sales) print cat, sales[cat]}' sales.csv

# Generate HTML table from data
awk 'BEGIN {print "<table>"} 
     {print "<tr><td>"$1"</td><td>"$2"</td></tr>"} 
     END {print "</table>"}' data.txt
```

---

## 4. `cut`, `sort`, `uniq` — Quick Processing

### `cut` — Extract Columns

```bash
# Cut by character position
cut -c1-10 file.txt                 # First 10 characters

# Cut by field (delimiter)
cut -d: -f1 /etc/passwd             # First field, colon delimiter
cut -d, -f2,4 data.csv              # Fields 2 and 4 from CSV

# Cut from field 3 to end
cut -d: -f3- /etc/passwd
```

### `sort` — Sort Data

```bash
# Sort alphabetically
sort file.txt

# Sort numerically
sort -n numbers.txt

# Sort in reverse
sort -r file.txt

# Sort by specific column (3rd field)
sort -t: -k3 -n /etc/passwd

# Sort human-readable sizes
du -sh */ | sort -rh

# Sort and remove duplicates
sort -u file.txt

# Sort by multiple keys
sort -t, -k2,2 -k3,3n data.csv     # Sort by col 2, then col 3 numerically
```

### `uniq` — Filter Duplicates

```bash
# Remove adjacent duplicates (file must be sorted first)
sort file.txt | uniq

# Count occurrences
sort file.txt | uniq -c

# Show only duplicate lines
sort file.txt | uniq -d

# Show only unique lines (appear once)
sort file.txt | uniq -u

# Ignore case when comparing
sort file.txt | uniq -ic
```

---

## 5. `tr` — Translate Characters

```bash
# Convert to uppercase
echo "hello world" | tr 'a-z' 'A-Z'

# Convert to lowercase
echo "HELLO WORLD" | tr 'A-Z' 'a-z'

# Replace spaces with newlines (one word per line)
echo "one two three" | tr ' ' '\n'

# Delete specific characters
echo "hello 123 world" | tr -d '0-9'         # Remove digits

# Squeeze repeated characters
echo "hello     world" | tr -s ' '           # Single space

# Replace non-alphanumeric characters
echo "hello@world#2024!" | tr -c 'a-zA-Z0-9\n' '_'

# Remove carriage returns (Windows → Linux line endings)
tr -d '\r' < windows_file.txt > linux_file.txt
```

---

## 6. Practical Combination Examples

```bash
# Frequency analysis of words in a file
cat file.txt | tr 'A-Z' 'a-z' | tr -s ' ' '\n' | sort | uniq -c | sort -rn | head -20

# Extract and sort unique email addresses
grep -oE '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}' file.txt | sort -u

# CSV: Get summary of column values
awk -F, 'NR>1 {print $2}' data.csv | sort | uniq -c | sort -rn

# Parse JSON-like key-value pairs
grep -oP '"name"\s*:\s*"\K[^"]+' data.json

# Convert a file to comma-separated values
paste -sd, file.txt

# Combine multiple files column-wise
paste file1.txt file2.txt file3.txt

# Number all non-empty lines in a file
grep -n '.' file.txt
```

---

## 💡 Tips

- Always test `sed -i` commands without `-i` first to preview changes
- `awk` is more powerful than `cut` for complex field extraction
- Use `grep -P` for Perl-compatible regex (more powerful patterns)
- Chain `sort | uniq -c | sort -rn` for frequency analysis — you'll use it all the time
- When processing CSVs with `awk`, use `-F,` but be aware of quoted fields with commas

---

[← Back to Index](../README.md) | [Next: System Troubleshooting Guide →](05-system-troubleshooting.md)
