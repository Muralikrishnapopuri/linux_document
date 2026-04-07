# 🔐 File Permissions & Ownership

Control who can read, write, and execute files.

---

## Understanding Permission Notation

When you run `ls -l`, the first column shows permissions:

```
-rwxr-xr-- 1 user group 4096 Jan 01 12:00 script.sh
```

### Breakdown

```
-   rwx   r-x   r--
│    │     │     │
│    │     │     └── Others (everyone else)
│    │     └──────── Group
│    └────────────── Owner (user)
└─────────────────── File type (- = file, d = directory, l = link)
```

| Symbol | Permission |
|--------|-----------|
| `r` | Read (4) |
| `w` | Write (2) |
| `x` | Execute (1) |
| `-` | No permission (0) |

---

## Numeric (Octal) Permission Values

| Number | Permission | Meaning |
|--------|-----------|---------|
| `0` | `---` | No permission |
| `1` | `--x` | Execute only |
| `2` | `-w-` | Write only |
| `3` | `-wx` | Write + Execute |
| `4` | `r--` | Read only |
| `5` | `r-x` | Read + Execute |
| `6` | `rw-` | Read + Write |
| `7` | `rwx` | Read + Write + Execute |

### Common Permission Sets

| Octal | Symbolic | Typical Use |
|-------|----------|-------------|
| `755` | `rwxr-xr-x` | Executable scripts, directories |
| `644` | `rw-r--r--` | Regular files |
| `700` | `rwx------` | Private scripts |
| `600` | `rw-------` | Private files (SSH keys) |
| `777` | `rwxrwxrwx` | Full access for everyone (⚠️ avoid) |

---

## 1. `chmod` — Change File Permissions

### Using Numeric Mode

```bash
chmod 755 script.sh          # Owner: rwx, Group: r-x, Others: r-x
chmod 644 file.txt           # Owner: rw-, Group: r--, Others: r--
chmod 600 id_rsa             # Owner: rw-, no one else
chmod 777 public_dir/        # Full permissions (use sparingly!)
```

### Using Symbolic Mode

```bash
# Syntax: chmod [who][operator][permission] file
# who:      u (user/owner), g (group), o (others), a (all)
# operator: + (add), - (remove), = (set exactly)

chmod u+x script.sh          # Add execute for owner
chmod g-w file.txt           # Remove write for group
chmod o-rwx private.txt      # Remove all for others
chmod a+r public.txt         # Add read for everyone
chmod u=rwx,g=rx,o=r file    # Set exact permissions
```

### Recursive Permissions

```bash
chmod -R 755 mydir/          # Apply recursively to directory
chmod -R u+w project/        # Add write for owner recursively
```

---

## 2. `chown` — Change File Ownership

```bash
# Change owner
sudo chown alice file.txt

# Change owner and group
sudo chown alice:developers file.txt

# Change only group (with colon)
sudo chown :developers file.txt

# Recursive ownership change
sudo chown -R alice:developers project/
```

---

## 3. `chgrp` — Change Group Ownership

```bash
chgrp developers file.txt          # Change group
chgrp -R developers project/       # Recursive group change
```

---

## 4. `umask` — Default Permission Mask

Controls default permissions for newly created files and directories.

```bash
umask                    # Show current umask
umask 022                # Set umask (default on most systems)
umask 077                # Restrictive: only owner gets access
```

### How umask Works

```
Files:       666 - umask = default permissions
Directories: 777 - umask = default permissions

Example with umask 022:
  Files:       666 - 022 = 644 (rw-r--r--)
  Directories: 777 - 022 = 755 (rwxr-xr-x)
```

---

## 5. Special Permissions

### SUID (Set User ID) — `4`

File runs with owner's permissions regardless of who executes it.

```bash
chmod 4755 program         # Set SUID
chmod u+s program          # Set SUID (symbolic)
ls -l program              # Shows: -rwsr-xr-x
```

### SGID (Set Group ID) — `2`

File runs with group's permissions. On directories, new files inherit the group.

```bash
chmod 2755 shared_dir/     # Set SGID on directory
chmod g+s shared_dir/      # Set SGID (symbolic)
ls -ld shared_dir/         # Shows: drwxr-sr-x
```

### Sticky Bit — `1`

On directories, only the file owner can delete their files (used on `/tmp`).

```bash
chmod 1777 /tmp            # Set sticky bit
chmod +t shared_dir/       # Set sticky bit (symbolic)
ls -ld /tmp                # Shows: drwxrwxrwt
```

---

## 6. `getfacl` / `setfacl` — Access Control Lists (ACL)

More fine-grained permissions beyond standard user/group/others.

```bash
# View ACL
getfacl file.txt

# Grant read/write to specific user
setfacl -m u:alice:rw file.txt

# Grant read to specific group
setfacl -m g:developers:r file.txt

# Remove ACL entry
setfacl -x u:alice file.txt

# Remove all ACLs
setfacl -b file.txt
```

---

## 💡 Tips

- Never use `chmod 777` in production — it's a security risk
- SSH keys (`~/.ssh/id_rsa`) must be `chmod 600` or SSH will refuse to use them
- Use `ls -la` to verify permissions after changes
- `chmod` affects files, `chown` affects ownership — you often need both
- The `sudo` command is needed for `chown` unless you own the file

---

[← Previous: File Viewing](03-file-viewing.md) | [Back to Index](../README.md) | [Next: Search & Find →](05-search-and-find.md)
