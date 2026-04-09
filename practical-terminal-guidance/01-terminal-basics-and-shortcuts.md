# ⌨️ Terminal Basics & Keyboard Shortcuts

Master the terminal with essential shortcuts and navigation tricks.

---

## 1. Opening the Terminal

### Ubuntu / Debian

| Method | How |
|--------|-----|
| Keyboard shortcut | `Ctrl + Alt + T` |
| Application menu | Search for "Terminal" in Activities |
| Right-click desktop | "Open Terminal Here" (if enabled) |
| File Manager | `Ctrl + L` → type `bash` |

### Common Terminal Emulators

| Terminal | Description |
|----------|-------------|
| **GNOME Terminal** | Default on Ubuntu/GNOME |
| **Konsole** | Default on KDE |
| **xterm** | Lightweight classic terminal |
| **Terminator** | Split-pane terminal emulator |
| **Alacritty** | GPU-accelerated, fast |
| **Kitty** | Modern, feature-rich |

---

## 2. Essential Keyboard Shortcuts

### Cursor Movement

| Shortcut | Action |
|----------|--------|
| `Ctrl + A` | Move cursor to beginning of line |
| `Ctrl + E` | Move cursor to end of line |
| `Alt + F` | Move forward one word |
| `Alt + B` | Move backward one word |
| `Ctrl + F` | Move forward one character |
| `Ctrl + B` | Move backward one character |

### Text Editing

| Shortcut | Action |
|----------|--------|
| `Ctrl + U` | Cut everything before cursor |
| `Ctrl + K` | Cut everything after cursor |
| `Ctrl + W` | Cut word before cursor |
| `Alt + D` | Cut word after cursor |
| `Ctrl + Y` | Paste (yank) previously cut text |
| `Ctrl + _` | Undo last edit |
| `Ctrl + XX` | Toggle between start and current cursor position |

### Process Control

| Shortcut | Action |
|----------|--------|
| `Ctrl + C` | Kill / cancel current command |
| `Ctrl + Z` | Suspend current command (send to background) |
| `Ctrl + D` | Close terminal / send EOF |
| `Ctrl + S` | Freeze terminal output (pause scrolling) |
| `Ctrl + Q` | Unfreeze terminal output |

### Screen Control

| Shortcut | Action |
|----------|--------|
| `Ctrl + L` | Clear the screen |
| `Shift + PgUp` | Scroll up in terminal output |
| `Shift + PgDn` | Scroll down in terminal output |
| `Ctrl + Shift + C` | Copy selected text |
| `Ctrl + Shift + V` | Paste text |

---

## 3. Command History Navigation

```bash
# Navigate through command history
↑ / ↓                  # Cycle through previous commands

# Search command history interactively
Ctrl + R               # Start reverse search
# Type part of the command → it finds matches
# Press Ctrl+R again → find next match
# Press Enter → execute the found command
# Press Esc → keep command but don't execute

# View full command history
history

# Run a specific command from history
!42                    # Run command #42 from history
!!                     # Repeat the last command
!ls                    # Run the last command starting with "ls"
!$                     # Use the last argument of the previous command
```

### Practical History Examples

```bash
# Search history for a specific command
history | grep "docker"

# Clear command history
history -c

# Run the last command with sudo
sudo !!

# Reuse the last argument from previous command
mkdir /tmp/mydir
cd !$                  # cd /tmp/mydir

# Edit and run a previous command
fc                     # Opens last command in default editor
```

---

## 4. Tab Completion

Tab completion saves time and prevents typos.

```bash
# Complete a command name
sys<Tab>               # Completes to "systemctl" (if unique)

# Complete a file/directory name
cd /ho<Tab>            # Completes to /home/
cd /home/<Tab><Tab>    # Shows all directories in /home/

# Complete options (with bash-completion installed)
git <Tab><Tab>         # Shows all git subcommands
docker run --<Tab><Tab>  # Shows all available options
```

### Install Enhanced Tab Completion

```bash
sudo apt install bash-completion
source /etc/bash_completion
```

---

## 5. Multiple Commands on One Line

| Operator | Description | Example |
|----------|-------------|---------|
| `;` | Run commands sequentially (regardless of success) | `mkdir test; cd test; touch file.txt` |
| `&&` | Run next only if previous succeeds | `make && make install` |
| `||` | Run next only if previous fails | `ping -c1 google.com || echo "No internet"` |
| `&` | Run command in background | `sleep 60 &` |

### Practical Examples

```bash
# Update and upgrade in one go
sudo apt update && sudo apt upgrade -y

# Create a project structure in one line
mkdir -p project/{src,docs,tests} && touch project/README.md

# Try to connect, show error if fails
ssh server || echo "Connection failed"

# Run long task in background
tar -czf backup.tar.gz /data &
```

---

## 6. Terminal Multiplexing with `tmux`

### Installation

```bash
sudo apt install tmux
```

### Essential tmux Commands

```bash
# Start a new session
tmux

# Start a named session
tmux new -s mysession

# Detach from session
Ctrl + B, then D

# List sessions
tmux ls

# Attach to a session
tmux attach -t mysession

# Kill a session
tmux kill-session -t mysession
```

### Window & Pane Management

| Shortcut | Action |
|----------|--------|
| `Ctrl+B, c` | Create new window |
| `Ctrl+B, n` | Next window |
| `Ctrl+B, p` | Previous window |
| `Ctrl+B, %` | Split vertically |
| `Ctrl+B, "` | Split horizontally |
| `Ctrl+B, arrow` | Navigate between panes |
| `Ctrl+B, x` | Close current pane |

---

## 💡 Tips

- Master `Ctrl+R` — it's the fastest way to find and rerun old commands
- Use `Ctrl+A` and `Ctrl+E` instead of Home/End keys — they work everywhere
- `Ctrl+U` + `Ctrl+Y` is powerful for temporarily editing a long command
- Use `tmux` when working on remote servers — it survives disconnections
- Double-click selects a word, triple-click selects a line in most terminals

---

[← Back to Index](../README.md) | [Next: Command Chaining & Workflows →](02-command-chaining-workflows.md)
