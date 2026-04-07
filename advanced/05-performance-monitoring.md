# 📊 Performance Monitoring

Monitor CPU, memory, disk I/O, and system performance.

---

## 1. `htop` — Interactive Process Viewer

```bash
sudo apt install htop
htop
```

### htop Navigation

| Key | Action |
|-----|--------|
| `F1` | Help |
| `F2` | Setup |
| `F3` / `/` | Search |
| `F4` | Filter |
| `F5` | Tree view |
| `F6` | Sort by column |
| `F9` | Kill process |
| `F10` / `q` | Quit |
| `u` | Filter by user |
| `M` | Sort by memory |
| `P` | Sort by CPU |
| `T` | Sort by time |

---

## 2. `top` — Classic Process Monitor

```bash
top                      # Launch
top -b -n 1              # Batch mode (one snapshot)
top -u alice             # Show user's processes
top -p 1234              # Monitor specific PID
```

Understanding the header:
```
load average: 0.5, 0.8, 1.2    ← 1-min, 5-min, 15-min averages
Tasks: 200 total, 1 running     ← Process counts
%Cpu: 15.3 us, 3.2 sy          ← User, system CPU usage
MiB Mem: 16000 total, 8000 free ← Memory stats
```

---

## 3. `vmstat` — Virtual Memory Statistics

```bash
vmstat                   # Single snapshot
vmstat 2                 # Update every 2 seconds
vmstat 2 10              # Update every 2s, 10 times
vmstat -s                # Memory statistics summary
vmstat -d                # Disk statistics
```

### Output Columns

| Column | Description |
|--------|------------|
| `r` | Processes waiting to run |
| `b` | Processes in uninterruptible sleep |
| `swpd` | Virtual memory used |
| `free` | Idle memory |
| `si/so` | Swap in/out |
| `bi/bo` | Blocks read/written to disk |
| `us` | User CPU time |
| `sy` | System CPU time |
| `id` | Idle CPU time |
| `wa` | Wait I/O time |

---

## 4. `iostat` — I/O Statistics

```bash
# Install
sudo apt install sysstat

iostat                   # Basic I/O stats
iostat -x               # Extended stats
iostat -x 2              # Every 2 seconds
iostat -d               # Disk stats only
iostat -c               # CPU stats only
```

---

## 5. `sar` — System Activity Reporter

```bash
# CPU usage
sar -u 2 5               # Every 2s, 5 times

# Memory usage
sar -r 2 5

# Disk I/O
sar -d 2 5

# Network
sar -n DEV 2 5

# Historical data
sar -u -f /var/log/sysstat/sa01    # From log file
```

---

## 6. `mpstat` — CPU Statistics Per Core

```bash
mpstat                   # Overall CPU stats
mpstat -P ALL            # All individual CPU cores
mpstat -P ALL 2          # Every 2 seconds
```

---

## 7. `free` — Memory Usage

```bash
free -h                  # Human-readable
free -h -s 2             # Update every 2 seconds
free -m                  # In megabytes
watch -n 1 free -h       # Live monitoring
```

---

## 8. `strace` — System Call Tracer

Trace system calls made by a process. Great for debugging.

```bash
# Trace a command
strace ls

# Trace with timestamps
strace -t ls

# Trace specific system calls
strace -e open,read ls

# Trace a running process
strace -p 1234

# Count system calls
strace -c ls

# Save to file
strace -o output.txt ls

# Trace child processes too
strace -f ./script.sh
```

---

## 9. `lsof` — List Open Files

```bash
lsof                            # All open files
lsof -u alice                   # Files opened by user
lsof -i :80                     # What's using port 80
lsof -i :3000                   # What's using port 3000
lsof -p 1234                    # Files opened by PID
lsof +D /var/log                # Files open in directory
lsof -i TCP                     # TCP connections
lsof -i -P -n                   # Network connections (no DNS)
```

---

## 10. `dstat` — Versatile Resource Stats

```bash
# Install
sudo apt install dstat

dstat                    # Default view
dstat -c               # CPU only
dstat -d               # Disk only
dstat -n               # Network only
dstat -m               # Memory only
dstat --top-cpu         # Top CPU-consuming process
dstat --top-mem         # Top memory-consuming process
dstat -cdnm 2           # Combined, every 2s
```

---

## 11. `nmon` — Performance Monitor

```bash
# Install
sudo apt install nmon

nmon                    # Interactive mode
```

### Keys in nmon

| Key | Display |
|-----|---------|
| `c` | CPU |
| `m` | Memory |
| `d` | Disk |
| `n` | Network |
| `t` | Top processes |
| `q` | Quit |

---

## 12. Monitoring One-Liners

```bash
# CPU usage percentage
grep 'cpu ' /proc/stat | awk '{usage=($2+$4)*100/($2+$4+$5)} END {print usage "%"}'

# Memory usage percentage
free | awk '/Mem/{printf "%.1f%%\n", $3/$2*100}'

# Disk usage alert (if > 90%)
df -h | awk '$5+0 > 90 {print "WARNING:", $6, $5}'

# Network connections count
ss -s

# Top 5 CPU processes
ps aux --sort=-%cpu | head -6

# Top 5 memory processes
ps aux --sort=-%mem | head -6

# Watch disk I/O in real time
watch -n 1 "iostat -x | tail -5"
```

---

## 💡 Tips

- `htop` > `top` for interactive monitoring
- Use `vmstat` to quickly diagnose CPU vs I/O bottlenecks
- High `wa` in top/vmstat means disk I/O is the bottleneck
- Use `strace` to debug why a program isn't working
- `lsof -i :PORT` is the fastest way to find what's using a port

---

[← Previous: Firewall](04-firewall-security.md) | [Back to Index](../README.md) | [Next: Docker Basics →](06-docker-basics.md)
