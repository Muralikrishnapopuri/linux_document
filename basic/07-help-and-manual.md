# ❓ Help & Manual Pages

Learn how to get help for any command directly from the terminal.

---

## 1. `man` — Manual Pages

The most comprehensive help system in Linux.

```bash
man ls                   # Full manual for ls
man 5 passwd             # Section 5 (file formats) of passwd
man -k "copy"            # Search for manuals about "copy"
man -f ls                # Short description (same as whatis)
```

### Manual Sections

| Section | Content |
|---------|---------|
| 1 | User commands |
| 2 | System calls |
| 3 | Library functions |
| 4 | Special files |
| 5 | File formats |
| 6 | Games |
| 7 | Miscellaneous |
| 8 | System admin commands |

### Navigation Inside man

| Key | Action |
|-----|--------|
| `Space` | Next page |
| `b` | Previous page |
| `/pattern` | Search forward |
| `n` | Next search result |
| `q` | Quit |

---

## 2. `--help` Flag

Almost every command supports `--help` for a quick usage summary.

```bash
ls --help
grep --help
chmod --help
cp --help
```

This is usually shorter than `man` and shows just the options.

---

## 3. `help` — Built-in Shell Commands

For shell built-in commands (like `cd`, `echo`, `alias`), use `help`:

```bash
help cd
help echo
help alias
help export
help history
```

---

## 4. `whatis` — One-Line Description

```bash
whatis ls                # ls (1) - list directory contents
whatis grep              # grep (1) - print lines matching a pattern
whatis chmod             # chmod (1) - change file mode bits
```

---

## 5. `apropos` — Search Manual by Keyword

```bash
apropos "copy"           # Find commands related to copying
apropos "network"        # Network-related commands
apropos "disk"           # Disk-related commands
```

---

## 6. `info` — GNU Info Pages

More detailed than `man` for some GNU programs.

```bash
info coreutils           # Info on core utilities
info grep                # Detailed grep documentation
info bash                # Full bash documentation
```

---

## 7. `type` — What Kind of Command Is It?

```bash
type ls                  # alias, builtin, file, or function
type cd                  # cd is a shell builtin
type python3             # python3 is /usr/bin/python3
type ll                  # ll is aliased to 'ls -la'
```

---

## 8. `history` — Command History

```bash
history                  # Show all command history
history 20               # Show last 20 commands
history | grep "git"     # Search history for "git"
```

### History Shortcuts

| Shortcut | Action |
|----------|--------|
| `!!` | Repeat last command |
| `!n` | Repeat command number n |
| `!-2` | Repeat 2nd to last command |
| `!string` | Repeat last command starting with "string" |
| `Ctrl+R` | Reverse search through history |

### Clear History

```bash
history -c               # Clear current session history
history -w               # Write current history to file
```

---

## 9. `alias` — Create Command Shortcuts

```bash
# Create aliases
alias ll='ls -la'
alias gs='git status'
alias update='sudo apt update && sudo apt upgrade'

# View all aliases
alias

# Remove an alias
unalias ll
```

### Make Aliases Permanent

Add to `~/.bashrc` or `~/.bash_aliases`:

```bash
echo "alias ll='ls -la'" >> ~/.bashrc
source ~/.bashrc         # Reload to apply
```

---

## 💡 Tips

- Start with `--help` for a quick overview, use `man` for full details
- `Ctrl+R` is the fastest way to find a previous command
- Create aliases for commands you type frequently
- `man -k keyword` is great for discovering new commands

---

[← Previous: System Info](06-system-info.md) | [Back to Index](../README.md)
