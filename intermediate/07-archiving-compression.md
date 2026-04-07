# 🗜️ Archiving & Compression

Create archives, compress and extract files.

---

## 1. `tar` — Tape Archive

The most common archiving tool in Linux.

### Create Archives

```bash
# Create a .tar archive (no compression)
tar -cf archive.tar file1 file2 dir/

# Create compressed archives
tar -czf archive.tar.gz directory/           # gzip
tar -cjf archive.tar.bz2 directory/          # bzip2
tar -cJf archive.tar.xz directory/           # xz (best compression)

# With verbose output
tar -czvf archive.tar.gz directory/
```

### Extract Archives

```bash
# Extract .tar
tar -xf archive.tar

# Extract compressed archives
tar -xzf archive.tar.gz                      # gzip
tar -xjf archive.tar.bz2                     # bzip2
tar -xJf archive.tar.xz                      # xz

# Extract to specific directory
tar -xzf archive.tar.gz -C /destination/

# Extract with verbose output
tar -xzvf archive.tar.gz
```

### View Contents (Without Extracting)

```bash
tar -tf archive.tar                           # List files
tar -tzf archive.tar.gz                       # List gzipped archive
```

### tar Options Quick Reference

| Option | Meaning |
|--------|---------|
| `-c` | Create archive |
| `-x` | Extract archive |
| `-t` | List contents |
| `-f` | Specify filename |
| `-z` | gzip compression |
| `-j` | bzip2 compression |
| `-J` | xz compression |
| `-v` | Verbose output |
| `-C` | Change to directory |
| `--exclude` | Exclude files/patterns |

### Practical Examples

```bash
# Archive excluding node_modules
tar -czf project.tar.gz --exclude='node_modules' project/

# Archive excluding multiple patterns
tar -czf backup.tar.gz --exclude='*.log' --exclude='.git' project/

# Extract a single file from archive
tar -xzf archive.tar.gz path/to/file.txt
```

---

## 2. `gzip` / `gunzip` — GNU Zip

```bash
# Compress a file (replaces original)
gzip file.txt                    # Creates file.txt.gz

# Keep original file
gzip -k file.txt

# Decompress
gunzip file.txt.gz               # Or: gzip -d file.txt.gz

# Compress with level (1=fast, 9=best)
gzip -9 file.txt                 # Best compression

# View compressed file without extracting
zcat file.txt.gz
zless file.txt.gz
```

---

## 3. `bzip2` / `bunzip2` — Better Compression

```bash
bzip2 file.txt                   # Compress → file.txt.bz2
bzip2 -k file.txt               # Keep original
bunzip2 file.txt.bz2            # Decompress
bzip2 -9 file.txt               # Best compression
bzcat file.txt.bz2              # View without extracting
```

---

## 4. `xz` / `unxz` — Best Compression

```bash
xz file.txt                     # Compress → file.txt.xz
xz -k file.txt                  # Keep original
unxz file.txt.xz                # Decompress
xz -9 file.txt                  # Best compression (slower)
xzcat file.txt.xz               # View without extracting
```

---

## 5. `zip` / `unzip` — ZIP Archives

Cross-platform format (Windows compatible).

### Create ZIP

```bash
zip archive.zip file1 file2              # Zip files
zip -r archive.zip directory/            # Zip directory recursively
zip -e archive.zip file.txt              # Encrypted zip (prompts for password)
zip -9 archive.zip file.txt              # Best compression
zip archive.zip *.txt                    # Zip all .txt files
```

### Extract ZIP

```bash
unzip archive.zip                        # Extract here
unzip archive.zip -d /destination/       # Extract to directory
unzip -l archive.zip                     # List contents
unzip -o archive.zip                     # Overwrite without prompting
```

### Install if not present

```bash
sudo apt install zip unzip
```

---

## 6. `7z` — 7-Zip (Highest Compression)

```bash
# Install
sudo apt install p7zip-full

# Create archive
7z a archive.7z directory/

# Extract
7z x archive.7z

# List contents
7z l archive.7z

# Add password
7z a -p archive.7z directory/
```

---

## 7. Compression Comparison

| Format | Command | Compression | Speed | Compatibility |
|--------|---------|-------------|-------|---------------|
| `.gz` | `gzip` | Good | Fast | Linux standard |
| `.bz2` | `bzip2` | Better | Slower | Linux |
| `.xz` | `xz` | Best | Slowest | Linux |
| `.zip` | `zip` | Good | Fast | Cross-platform |
| `.7z` | `7z` | Excellent | Slow | Cross-platform |

---

## 💡 Tips

- Use `tar.gz` for most Linux backup needs
- Use `.zip` when sharing with Windows users
- Use `xz` when disk space is critical and time isn't
- Remember: `tar` archives, `gzip/bzip2/xz` compresses — tar combines both
- Always test your archive with `-t` (tar) or `-l` (zip) before deleting originals

---

[← Previous: Package Management](06-package-management.md) | [Back to Index](../README.md) | [Next: I/O Redirection →](08-io-redirection-piping.md)
