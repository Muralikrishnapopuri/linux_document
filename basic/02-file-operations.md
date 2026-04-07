# 📄 File Operations

Create, copy, move, rename, and delete files and directories.

---

## 1. `touch` — Create Empty Files / Update Timestamps

```bash
touch file.txt                 # Create a new empty file
touch file1.txt file2.txt      # Create multiple files
touch -t 202401011200 file.txt # Set specific timestamp
```

---

## 2. `mkdir` — Create Directories

```bash
mkdir mydir                    # Create a single directory
mkdir dir1 dir2 dir3           # Create multiple directories
mkdir -p parent/child/grandchild  # Create nested directories
mkdir -m 755 securedir         # Create with specific permissions
```

---

## 3. `cp` — Copy Files and Directories

### Basic Usage

```bash
cp source.txt destination.txt       # Copy a file
cp file.txt /home/user/Documents/   # Copy to a directory
```

### Common Options

| Option | Description | Example |
|--------|------------|---------|
| `-r` or `-R` | Copy directories recursively | `cp -r dir1 dir2` |
| `-i` | Prompt before overwriting | `cp -i file1 file2` |
| `-v` | Verbose (show what's being copied) | `cp -v file1 file2` |
| `-u` | Copy only when source is newer | `cp -u file1 file2` |
| `-p` | Preserve permissions and timestamps | `cp -p file1 file2` |
| `-a` | Archive (preserves everything, recursive) | `cp -a dir1 dir2` |

### Practical Examples

```bash
# Copy a directory and all its contents
cp -r project/ project_backup/

# Copy with verbose output
cp -rv src/ dest/

# Copy only newer files (useful for backups)
cp -u *.txt /backup/

# Copy preserving all attributes
cp -a /var/www/ /var/www_backup/
```

---

## 4. `mv` — Move or Rename Files and Directories

### Basic Usage

```bash
mv old_name.txt new_name.txt         # Rename a file
mv file.txt /home/user/Documents/    # Move to another directory
mv dir1/ /tmp/                        # Move a directory
```

### Common Options

| Option | Description | Example |
|--------|------------|---------|
| `-i` | Prompt before overwriting | `mv -i file1 file2` |
| `-v` | Verbose output | `mv -v *.txt /docs/` |
| `-n` | Do not overwrite existing files | `mv -n file1 file2` |
| `-u` | Move only if source is newer | `mv -u file1 file2` |

### Practical Examples

```bash
# Rename a file
mv report.txt final_report.txt

# Move multiple files to a directory
mv *.jpg *.png images/

# Move and rename
mv /tmp/data.csv /home/user/docs/dataset.csv

# Rename a directory
mv old_project/ new_project/
```

---

## 5. `rm` — Remove Files and Directories

### Basic Usage

```bash
rm file.txt                    # Remove a single file
rm file1.txt file2.txt         # Remove multiple files
```

### Common Options

| Option | Description | Example |
|--------|------------|---------|
| `-r` or `-R` | Remove directories recursively | `rm -r mydir/` |
| `-f` | Force removal (no confirmation) | `rm -f file.txt` |
| `-i` | Prompt before each removal | `rm -i *.txt` |
| `-v` | Verbose (show what's being deleted) | `rm -rv mydir/` |
| `-rf` | Force recursive removal (⚠️ dangerous) | `rm -rf dir/` |

### Practical Examples

```bash
# Remove a directory and all contents
rm -r old_project/

# Remove with confirmation
rm -i important_file.txt

# Remove all .log files
rm *.log

# Remove empty directory (safer alternative)
rmdir empty_dir/
```

> ⚠️ **WARNING**: `rm -rf /` will destroy your entire system. Always double-check paths before using `-rf`.

---

## 6. `rmdir` — Remove Empty Directories

```bash
rmdir empty_dir                # Remove single empty directory
rmdir dir1 dir2 dir3           # Remove multiple empty directories
rmdir -p a/b/c                 # Remove nested empty directories
```

---

## 7. `ln` — Create Links

### Symbolic (Soft) Links

```bash
ln -s /path/to/original link_name      # Create symbolic link
ln -s /usr/bin/python3 /usr/bin/python  # Common use case
```

### Hard Links

```bash
ln original.txt hardlink.txt            # Create hard link
```

### Key Differences

| Feature | Symbolic Link | Hard Link |
|---------|--------------|-----------|
| Cross filesystem | ✅ Yes | ❌ No |
| Link to directory | ✅ Yes | ❌ No |
| Original deleted | ❌ Link breaks | ✅ Still works |
| Inode | Different | Same |

---

## 8. `rename` — Bulk Rename Files

```bash
# Install if needed
sudo apt install rename

# Rename .txt to .md
rename 's/\.txt$/\.md/' *.txt

# Make filenames lowercase
rename 'y/A-Z/a-z/' *

# Add prefix to files
rename 's/^/backup_/' *.log
```

---

## 9. `file` — Determine File Type

```bash
file document.pdf        # Output: PDF document
file image.jpg           # Output: JPEG image data
file script.sh           # Output: Bourne-Again shell script
file /bin/ls             # Output: ELF 64-bit executable
```

---

## 10. `stat` — Display Detailed File Info

```bash
stat file.txt
```

**Output includes:**
- File size
- Inode number
- Number of links
- Access, modify, and change timestamps
- Permissions in octal and symbolic format

---

## 💡 Tips

- Always use `rm -i` when deleting important files for safety
- Use `cp -a` for backups to preserve all file attributes
- `mv` within the same filesystem is instant (just changes the pointer)
- Create aliases for safe defaults: `alias rm='rm -i'`

---

[← Previous: Navigation](01-file-system-navigation.md) | [Back to Index](../README.md) | [Next: File Viewing →](03-file-viewing.md)
