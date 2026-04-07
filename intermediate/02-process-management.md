# ⚙️ Process Management

Monitor, control, and manage running processes.

---

## 1. `ps` — Process Status

### Basic Usage

```bash
ps                       # Show processes for current terminal
ps aux                   # Show ALL running processes
ps -ef                   # Full-format listing of all processes
```

### Common Options

| Command | Description |
|---------|------------|
| `ps aux` | All processes with user info |
| `ps -ef` | All processes in full format |
| `ps aux --sort=-%mem` | Sort by memory usage (highest first) |
| `ps aux --sort=-%cpu` | Sort by CPU usage (highest first) |
| `ps -u username` | Processes by specific user |
| `ps -p 1234` | Info about specific PID |
| `ps -C nginx` | Processes by command name |

### Practical Examples

```bash
# Find a specific process
ps aux | grep nginx

# Top 10 memory-consuming processes
ps aux --sort=-%mem | head -11

# Top 10 CPU-consuming processes
ps aux --sort=-%cpu | head -11

# Show process tree
ps auxf
```

---

## 2. `top` — Real-Time Process Monitor

```bash
top                      # Launch interactive monitor
```

### Interactive Keys in `top`

| Key | Action |
|-----|--------|
| `q` | Quit |
| `M` | Sort by memory |
| `P` | Sort by CPU |
| `k` | Kill a process (enter PID) |
| `1` | Show individual CPU cores |
| `h` | Help |
| `u` | Filter by user |
| `f` | Choose display fields |

### Run Once (Batch Mode)

```bash
top -b -n 1              # Run once and print output
top -b -n 1 | head -20   # Top 20 lines
```

---

## 3. `htop` — Enhanced Process Viewer

Interactive, colorful process viewer (better than `top`).

```bash
# Install
sudo apt install htop

# Run
htop
```

### Navigation

| Key | Action |
|-----|--------|
| `F3` / `/` | Search |
| `F5` | Tree view |
| `F6` | Sort by column |
| `F9` | Kill process |
| `F10` / `q` | Quit |

---

## 4. `kill` — Send Signals to Processes

### Common Signals

| Signal | Number | Meaning |
|--------|--------|---------|
| `SIGHUP` | 1 | Hangup (restart) |
| `SIGINT` | 2 | Interrupt (Ctrl+C) |
| `SIGKILL` | 9 | Force kill (cannot be caught) |
| `SIGTERM` | 15 | Graceful termination (default) |
| `SIGSTOP` | 19 | Stop/pause process |
| `SIGCONT` | 18 | Continue stopped process |

### Usage

```bash
kill 1234                # Send SIGTERM (graceful) to PID 1234
kill -9 1234             # Force kill PID 1234
kill -SIGKILL 1234       # Same as above
kill -15 1234            # SIGTERM (default)
kill -1 1234             # Send hangup (restart)
```

---

## 5. `killall` — Kill Processes by Name

```bash
killall firefox          # Kill all firefox processes
killall -9 python3       # Force kill all python3 processes
killall -u alice         # Kill all processes by user alice
killall -i nginx         # Interactive (confirm each kill)
```

---

## 6. `pkill` / `pgrep` — Pattern-Based Process Operations

```bash
# Find process IDs by name pattern
pgrep nginx              # List PIDs of nginx processes
pgrep -l nginx           # PIDs with process names
pgrep -u alice           # PIDs of alice's processes

# Kill by name pattern
pkill firefox            # Kill processes matching "firefox"
pkill -9 -f "python script.py"  # Force kill by full command
```

---

## 7. Background & Foreground Jobs

### Running in Background

```bash
# Start a command in the background
sleep 100 &

# Move a running command to background
# 1. Press Ctrl+Z (pause/stop the process)
# 2. Then type:
bg                       # Resume in background
```

### Job Control

```bash
jobs                     # List all background jobs
fg                       # Bring last background job to foreground
fg %1                    # Bring job #1 to foreground
bg %2                    # Resume job #2 in background
```

---

## 8. `nohup` — Run Command Immune to Hangups

Keeps a command running even after you close the terminal.

```bash
nohup long_running_script.sh &
nohup python3 server.py > output.log 2>&1 &
```

Output is saved to `nohup.out` by default.

---

## 9. `screen` / `tmux` — Terminal Multiplexers

### screen

```bash
screen                   # Start new session
screen -S mywork         # Named session
screen -ls               # List sessions
screen -r mywork         # Reattach to session
# Ctrl+A, D              # Detach from session
```

### tmux

```bash
tmux                     # Start new session
tmux new -s mywork       # Named session
tmux ls                  # List sessions
tmux attach -t mywork    # Attach to session
# Ctrl+B, D              # Detach from session
```

---

## 10. `nice` / `renice` — Process Priority

Priority ranges from -20 (highest) to 19 (lowest). Default is 0.

```bash
# Start a process with low priority
nice -n 10 ./heavy_task.sh

# Start with high priority (needs sudo)
sudo nice -n -5 ./important_task.sh

# Change priority of running process
renice 10 -p 1234        # Set PID 1234 to priority 10
sudo renice -5 -p 1234   # Set higher priority
```

---

## 11. `watch` — Run Command Repeatedly

```bash
watch df -h              # Refresh disk usage every 2s
watch -n 5 free -h       # Refresh memory every 5s
watch -d ls -la          # Highlight changes
watch -n 1 "ps aux | wc -l"  # Process count every second
```

---

## 💡 Tips

- Use `kill -15` (SIGTERM) first; only use `kill -9` (SIGKILL) as last resort
- `htop` is more user-friendly than `top` — install it
- Use `nohup` or `tmux` for long-running tasks on remote servers
- `Ctrl+Z` pauses a process; `bg` resumes it in the background

---

[← Previous: Text Processing](01-text-processing.md) | [Back to Index](../README.md) | [Next: Disk & Storage →](03-disk-and-storage.md)
