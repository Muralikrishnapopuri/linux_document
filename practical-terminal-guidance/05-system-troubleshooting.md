# 🔧 System Troubleshooting Guide

Diagnose and fix common Linux issues from the terminal.

---

## 1. System Won't Boot / Slow Boot

### Check Boot Time

```bash
# Total boot time breakdown
systemd-analyze

# Time taken by each service
systemd-analyze blame | head -20

# Visual boot chart (generates SVG)
systemd-analyze plot > boot_chart.svg

# Check for failed services
systemctl --failed

# View boot logs
journalctl -b           # Current boot
journalctl -b -1        # Previous boot
```

### Fix Slow Boot

```bash
# Disable unnecessary services
sudo systemctl disable bluetooth.service
sudo systemctl disable cups.service

# Mask services you never need
sudo systemctl mask ModemManager.service

# Check if networking is causing delays
systemd-analyze critical-chain

# Reduce GRUB timeout
sudo sed -i 's/GRUB_TIMEOUT=.*/GRUB_TIMEOUT=2/' /etc/default/grub
sudo update-grub
```

---

## 2. Disk Space Issues

### Diagnose

```bash
# Check overall disk usage
df -h

# Find what's eating space (top-level)
sudo du -sh /* 2>/dev/null | sort -rh | head -15

# Find large files (>100MB)
sudo find / -type f -size +100M -exec ls -lh {} \; 2>/dev/null

# Check inode usage (too many small files)
df -i
```

### Fix — Free Up Space

```bash
# Clean package cache
sudo apt clean
sudo apt autoclean

# Remove unused packages
sudo apt autoremove

# Remove old kernels (keep current)
sudo apt autoremove --purge

# Clean journal logs (keep last 3 days)
sudo journalctl --vacuum-time=3d

# Find and remove old log files
sudo find /var/log -name "*.gz" -mtime +30 -delete

# Empty trash
rm -rf ~/.local/share/Trash/*

# Find and remove core dumps
find / -name "core" -type f -delete 2>/dev/null

# Check Docker disk usage
docker system df
docker system prune -a         # Remove unused images, containers, volumes
```

---

## 3. High CPU Usage

### Diagnose

```bash
# Real-time process monitor
top
htop                            # More user-friendly (install: sudo apt install htop)

# Sort by CPU in top: press 'P'
# Sort by Memory in top: press 'M'

# Find top CPU-consuming processes
ps aux --sort=-%cpu | head -10

# Monitor specific process
top -p $(pgrep -f "process_name")

# Check system load average
uptime
# Load > number of CPU cores = overloaded

# Count CPU cores
nproc
```

### Fix

```bash
# Kill a runaway process
kill <PID>                      # Graceful
kill -9 <PID>                   # Force kill

# Kill all processes by name
killall firefox

# Kill processes matching a pattern
pkill -f "python runaway_script"

# Reduce process priority (nice)
nice -n 19 ./heavy_process      # Start with lowest priority
renice 19 -p <PID>              # Change running process priority

# Limit CPU for a process
sudo apt install cpulimit
cpulimit -p <PID> -l 50        # Limit to 50% CPU
```

---

## 4. Memory Issues

### Diagnose

```bash
# Check memory usage
free -h

# Detailed memory info
cat /proc/meminfo

# Top memory-consuming processes
ps aux --sort=-%mem | head -10

# Check swap usage
swapon --show

# Check for memory leaks (watch over time)
watch -n 2 'free -h'

# Detailed per-process memory
smem -r -k | head -20          # Install: sudo apt install smem
```

### Fix

```bash
# Clear filesystem cache (safe to run)
sudo sync && sudo echo 3 > /proc/sys/vm/drop_caches

# Kill memory-hungry process
kill $(ps aux --sort=-%mem | awk 'NR==2{print $2}')

# Add swap file (if running out of RAM)
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
# Make permanent:
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

# Adjust swappiness (lower = prefer RAM)
sudo sysctl vm.swappiness=10
```

---

## 5. Network Troubleshooting

### Check Connectivity

```bash
# Check if interface is up
ip addr show
ip link show

# Test internet connectivity
ping -c 4 8.8.8.8              # IP connectivity
ping -c 4 google.com           # DNS + connectivity

# Check DNS resolution
nslookup google.com
dig google.com
host google.com

# Check routing
ip route show
traceroute google.com           # Or: tracepath google.com

# Check DNS configuration
cat /etc/resolv.conf
```

### Fix Common Network Issues

```bash
# Restart networking
sudo systemctl restart NetworkManager
# Or:
sudo systemctl restart systemd-networkd

# Restart a specific interface
sudo ip link set eth0 down
sudo ip link set eth0 up

# Flush DNS cache
sudo systemd-resolve --flush-caches
# Or:
sudo resolvectl flush-caches

# Manually set DNS
echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf

# Check if a port is reachable
nc -zv hostname 80
curl -v telnet://hostname:443

# Check firewall rules
sudo ufw status verbose
sudo iptables -L -n
```

### Check Ports & Connections

```bash
# List all listening ports
sudo ss -tlnp

# Check if a specific port is in use
sudo ss -tlnp | grep :8080

# List established connections
ss -tn state established

# Find which process is using a port
sudo lsof -i :80
sudo fuser 80/tcp
```

---

## 6. Package / Software Issues

### Broken Packages

```bash
# Fix broken dependencies
sudo apt --fix-broken install

# Reconfigure packages
sudo dpkg --configure -a

# Force reinstall a package
sudo apt install --reinstall package_name

# Remove a broken package
sudo dpkg --remove --force-remove-reinstreq package_name

# Clean and rebuild package cache
sudo apt clean
sudo apt update
```

### Repository Issues

```bash
# Check and fix GPG key issues
sudo apt-key list
# For modern Ubuntu (22.04+):
# Keys are now in /etc/apt/keyrings/

# Remove a problematic PPA
sudo add-apt-repository --remove ppa:problematic/ppa

# Check for duplicate sources
grep -r "^deb " /etc/apt/sources.list /etc/apt/sources.list.d/

# List installed packages
dpkg --list | grep "package_name"
apt list --installed | grep "package_name"

# Check which package owns a file
dpkg -S /usr/bin/python3
```

---

## 7. Permission & Access Issues

```bash
# Check file ownership and permissions
ls -la /path/to/file
stat /path/to/file

# "Permission denied" — common fixes
# 1. Check if you have read/write/execute access
namei -l /path/to/file

# 2. Run with sudo if system file
sudo cat /etc/shadow

# 3. Fix ownership
sudo chown $USER:$USER /path/to/file

# 4. Fix permissions
chmod 755 /path/to/directory
chmod 644 /path/to/file

# SSH permission issues
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub
chmod 600 ~/.ssh/authorized_keys

# "Text file busy" error
fuser /path/to/file              # Find who's using it
```

---

## 8. Log Analysis for Troubleshooting

```bash
# View system logs
journalctl -xe                   # Recent logs with explanations
journalctl -f                    # Follow new logs in real-time

# Filter by service
journalctl -u nginx.service
journalctl -u ssh.service --since "1 hour ago"

# Filter by priority (errors and above)
journalctl -p err

# Filter by time range
journalctl --since "2024-01-01" --until "2024-01-02"
journalctl --since "10 minutes ago"

# Check authentication logs
sudo cat /var/log/auth.log | tail -20

# Check kernel messages
dmesg | tail -30
dmesg -T | grep -i error          # With timestamps

# Monitor all logs in real-time
sudo tail -f /var/log/syslog
```

---

## 9. Quick Diagnostic Script

Copy and run this to get a system health snapshot:

```bash
echo "===== SYSTEM INFO ====="
uname -a
echo ""
echo "===== UPTIME & LOAD ====="
uptime
echo ""
echo "===== MEMORY ====="
free -h
echo ""
echo "===== DISK USAGE ====="
df -h | grep -v tmpfs
echo ""
echo "===== TOP PROCESSES (CPU) ====="
ps aux --sort=-%cpu | head -6
echo ""
echo "===== TOP PROCESSES (MEM) ====="
ps aux --sort=-%mem | head -6
echo ""
echo "===== FAILED SERVICES ====="
systemctl --failed
echo ""
echo "===== NETWORK ====="
ip -brief addr show
echo ""
echo "===== LISTENING PORTS ====="
sudo ss -tlnp 2>/dev/null | head -10
```

---

## 💡 Tips

- Always check logs first: `journalctl -xe` is your best friend
- Use `dmesg` for hardware-related issues (USB, disk, driver errors)
- When something breaks after an update, check `apt log`: `/var/log/apt/history.log`
- For persistent issues, check if the problem exists in a new user account
- `strace -p <PID>` shows what system calls a process is making (great for debugging hangs)

---

[← Back to Index](../README.md) | [Next: Productivity Hacks →](06-productivity-hacks.md)
