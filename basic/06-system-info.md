# 🖥️ System Information

Check system details, hardware specs, and runtime status.

---

## 1. `uname` — System Information

```bash
uname                    # Kernel name (e.g., Linux)
uname -a                 # All system info
uname -r                 # Kernel version
uname -m                 # Machine architecture (x86_64, aarch64)
uname -n                 # Hostname
uname -s                 # Kernel name
uname -o                 # Operating system
```

**Example output of `uname -a`:**
```
Linux mypc 5.15.0-91-generic #101-Ubuntu SMP x86_64 GNU/Linux
```

---

## 2. `hostname` — Display or Set Hostname

```bash
hostname                 # Show hostname
hostname -I              # Show IP address(es)
hostname -f              # Fully qualified domain name
```

---

## 3. `whoami` — Current User

```bash
whoami                   # Display currently logged-in username
```

---

## 4. `id` — User Identity Details

```bash
id                       # UID, GID, and groups of current user
id alice                 # Info for a specific user
id -u                    # Just the user ID
id -g                    # Just the group ID
id -Gn                   # All group names
```

---

## 5. `uptime` — System Uptime

```bash
uptime                   # How long the system has been running
uptime -p                # Pretty format (e.g., "up 3 days, 4 hours")
uptime -s                # System start time
```

---

## 6. `date` — Date and Time

```bash
date                     # Current date and time
date +"%Y-%m-%d"         # Format: 2024-01-15
date +"%H:%M:%S"         # Format: 14:30:00
date +"%A, %B %d, %Y"   # Format: Monday, January 15, 2024
date -u                  # UTC time
```

### Common Format Codes

| Code | Meaning | Example |
|------|---------|---------|
| `%Y` | 4-digit year | 2024 |
| `%m` | Month (01-12) | 01 |
| `%d` | Day (01-31) | 15 |
| `%H` | Hour (00-23) | 14 |
| `%M` | Minute (00-59) | 30 |
| `%S` | Second (00-59) | 00 |
| `%A` | Full weekday | Monday |
| `%B` | Full month | January |

---

## 7. `cal` — Display Calendar

```bash
cal                      # Current month
cal 2024                 # Full year
cal 3 2024               # March 2024
cal -3                   # Previous, current, and next month
```

---

## 8. `free` — Memory Usage

```bash
free                     # Memory in kilobytes
free -h                  # Human-readable (MB, GB)
free -g                  # In gigabytes
free -m                  # In megabytes
```

**Output columns:**
| Column | Description |
|--------|------------|
| `total` | Total installed RAM |
| `used` | Memory in use |
| `free` | Completely unused memory |
| `shared` | Shared memory |
| `buff/cache` | Memory used for buffers/cache |
| `available` | Memory available for new processes |

---

## 9. `lscpu` — CPU Information

```bash
lscpu                   # Detailed CPU information
```

Shows: architecture, cores, threads, model name, clock speed, cache sizes.

---

## 10. `lsb_release` — Distribution Information

```bash
lsb_release -a           # Full distribution info
lsb_release -d           # Description only
cat /etc/os-release      # Alternative method
```

---

## 11. `df` — Disk Space Usage (Quick View)

```bash
df -h                    # Human-readable disk usage
df -h /                  # Usage of root partition
```

---

## 12. `lsblk` — List Block Devices

```bash
lsblk                   # List all block devices (disks, partitions)
lsblk -f                # Show filesystem info
```

---

## 13. `lshw` — Detailed Hardware Info

```bash
sudo lshw                # Complete hardware listing
sudo lshw -short         # Condensed summary
sudo lshw -C memory      # Memory hardware only
sudo lshw -C network     # Network hardware only
```

---

## 14. `lspci` / `lsusb` — PCI and USB Devices

```bash
lspci                    # List PCI devices (GPU, network card, etc.)
lspci | grep -i nvidia   # Find NVIDIA GPU
lsusb                    # List USB devices
```

---

## 15. `dmesg` — Kernel Messages

```bash
sudo dmesg                    # All kernel messages
sudo dmesg | tail -20         # Last 20 messages
sudo dmesg | grep -i error    # Kernel errors
sudo dmesg | grep -i usb      # USB-related messages
```

---

## 16. `env` / `printenv` — Environment Variables

```bash
env                      # Show all environment variables
printenv HOME            # Show specific variable
echo $PATH               # Print PATH variable
echo $HOME               # Print home directory
echo $USER               # Print current username
echo $SHELL              # Print current shell
```

---

## 💡 Tips

- `uname -a` and `lsb_release -a` together give you a complete system overview
- `free -h` is the fastest way to check memory usage
- Monitor memory in real-time: `watch -n 1 free -h`
- Check CPU load: `uptime` shows the load average for 1, 5, and 15 minutes

---

[← Previous: Search & Find](05-search-and-find.md) | [Back to Index](../README.md) | [Next: Help & Manual →](07-help-and-manual.md)
