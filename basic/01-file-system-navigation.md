# 📂 File System Navigation

Navigate through the Linux directory tree like a pro.

---

## 1. `pwd` — Print Working Directory

Shows the full path of the current directory you are in.

```bash
pwd
```

**Output:**
```
/home/murali-krishna
```

---

## 2. `ls` — List Directory Contents

Lists files and directories in the current location.

### Basic Usage

```bash
ls                  # List files in current directory
ls /etc             # List files in a specific directory
```

### Common Options

| Option | Description | Example |
|--------|------------|---------|
| `-l` | Long format (permissions, size, date) | `ls -l` |
| `-a` | Show hidden files (starting with `.`) | `ls -a` |
| `-la` | Long format + hidden files | `ls -la` |
| `-lh` | Long format with human-readable sizes | `ls -lh` |
| `-R` | Recursive listing (include subdirectories) | `ls -R` |
| `-t` | Sort by modification time (newest first) | `ls -lt` |
| `-S` | Sort by file size (largest first) | `ls -lS` |
| `-r` | Reverse sort order | `ls -lr` |
| `-d` | List directories themselves, not contents | `ls -d */` |
| `-1` | One file per line | `ls -1` |

### Practical Examples

```bash
# List only directories
ls -d */

# List files sorted by size (largest first)
ls -lhS

# List most recently modified files
ls -lt | head -10

# List files with a specific extension
ls *.txt

# Count number of files in a directory
ls -1 | wc -l
```

---

## 3. `cd` — Change Directory

Move between directories.

### Basic Usage

```bash
cd /home              # Go to an absolute path
cd Documents          # Go to a relative path (child directory)
```

### Special Shortcuts

| Command | Description |
|---------|------------|
| `cd` or `cd ~` | Go to your home directory |
| `cd ..` | Go up one level (parent directory) |
| `cd ../..` | Go up two levels |
| `cd -` | Go to the previous directory |
| `cd /` | Go to the root directory |

### Practical Examples

```bash
# Go to home directory
cd ~

# Navigate to Downloads
cd ~/Downloads

# Go back to the previous directory
cd -

# Go to the root of the file system
cd /

# Go up two levels then into a folder
cd ../../var/log
```

---

## 4. `tree` — Display Directory Tree

Shows a visual tree structure of directories and files.

### Installation (if not present)

```bash
sudo apt install tree
```

### Usage

```bash
tree                  # Show tree of current directory
tree /var/log         # Show tree of a specific directory
```

### Common Options

| Option | Description | Example |
|--------|------------|---------|
| `-L n` | Limit depth to n levels | `tree -L 2` |
| `-d` | Show directories only | `tree -d` |
| `-a` | Show hidden files too | `tree -a` |
| `-f` | Show full path for each file | `tree -f` |
| `-h` | Show human-readable sizes | `tree -h` |
| `--dirsfirst` | List directories before files | `tree --dirsfirst` |

### Practical Examples

```bash
# Show tree with 2 levels deep
tree -L 2

# Show only directories, 3 levels deep
tree -d -L 3

# Show tree with file sizes
tree -h -L 2
```

---

## 5. `pushd` / `popd` — Directory Stack

Navigate using a stack of directories.

```bash
# Save current directory and go to /var/log
pushd /var/log

# Save current and go to /etc
pushd /etc

# View the directory stack
dirs -v

# Pop back to the previous directory
popd
```

---

## 6. Linux Directory Structure Overview

| Path | Purpose |
|------|---------|
| `/` | Root of the entire file system |
| `/home` | User home directories |
| `/root` | Root user's home directory |
| `/etc` | System configuration files |
| `/var` | Variable data (logs, databases) |
| `/tmp` | Temporary files (cleared on reboot) |
| `/usr` | User programs and utilities |
| `/bin` | Essential command binaries |
| `/sbin` | System administration binaries |
| `/opt` | Optional / third-party software |
| `/dev` | Device files |
| `/proc` | Process and kernel info (virtual) |
| `/mnt` | Temporary mount points |
| `/media` | Removable media (USB, CD-ROM) |

---

## 💡 Tips

- Use `Tab` key to auto-complete directory and file names
- `cd -` is very useful for toggling between two directories
- Prefer absolute paths (`/home/user/file`) in scripts, relative paths (`./file`) in terminal
- `ls -la` is the most commonly used form of `ls`

---

[← Back to Index](../README.md) | [Next: File Operations →](02-file-operations.md)
