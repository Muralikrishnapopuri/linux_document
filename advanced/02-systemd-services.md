# 🔧 Systemd & Services

Manage system services, daemons, and the boot process.

---

## 1. `systemctl` — Service Manager

### Start, Stop, Restart Services

```bash
sudo systemctl start nginx          # Start a service
sudo systemctl stop nginx           # Stop a service
sudo systemctl restart nginx        # Restart a service
sudo systemctl reload nginx         # Reload config (no downtime)
sudo systemctl reload-or-restart nginx  # Reload if supported, else restart
```

### Enable/Disable at Boot

```bash
sudo systemctl enable nginx         # Start on boot
sudo systemctl disable nginx        # Don't start on boot
sudo systemctl enable --now nginx   # Enable AND start immediately
sudo systemctl disable --now nginx  # Disable AND stop immediately
```

### Check Status

```bash
systemctl status nginx              # Detailed status
systemctl is-active nginx           # Active or inactive
systemctl is-enabled nginx          # Enabled or disabled
systemctl is-failed nginx           # Failed or not
```

### List Services

```bash
systemctl list-units --type=service                # All loaded services
systemctl list-units --type=service --state=active # Active services
systemctl list-units --type=service --state=failed # Failed services
systemctl list-unit-files --type=service           # All service files
```

---

## 2. `journalctl` — System Logs

### View Logs

```bash
journalctl                                  # All logs
journalctl -u nginx                         # Logs for specific service
journalctl -u nginx --since today           # Today's logs
journalctl -u nginx --since "2024-01-01"    # Since a date
journalctl -u nginx --since "1 hour ago"    # Last hour
journalctl -u nginx -n 50                   # Last 50 lines
journalctl -u nginx -f                      # Follow (live tail)
```

### Filter by Priority

```bash
journalctl -p err                   # Errors only
journalctl -p warning               # Warnings and above
journalctl -p crit                  # Critical only
```

| Priority | Description |
|----------|------------|
| `emerg` | System is unusable |
| `alert` | Immediate action required |
| `crit` | Critical conditions |
| `err` | Errors |
| `warning` | Warnings |
| `notice` | Normal but significant |
| `info` | Informational |
| `debug` | Debug messages |

### Other Useful Options

```bash
journalctl -b                       # Logs since last boot
journalctl -b -1                    # Logs from previous boot
journalctl --disk-usage             # Disk space used by logs
sudo journalctl --vacuum-size=500M  # Reduce logs to 500MB
sudo journalctl --vacuum-time=7d    # Keep only 7 days of logs
journalctl -k                       # Kernel messages only
```

---

## 3. Creating a Custom Service

### Step 1: Create the Service File

```bash
sudo nano /etc/systemd/system/myapp.service
```

### Step 2: Write the Service Configuration

```ini
[Unit]
Description=My Application
After=network.target
Wants=network-online.target

[Service]
Type=simple
User=www-data
Group=www-data
WorkingDirectory=/opt/myapp
ExecStart=/usr/bin/python3 /opt/myapp/server.py
ExecReload=/bin/kill -HUP $MAINPID
Restart=on-failure
RestartSec=5
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
```

### Step 3: Enable and Start

```bash
sudo systemctl daemon-reload         # Reload systemd config
sudo systemctl enable myapp          # Enable at boot
sudo systemctl start myapp           # Start now
systemctl status myapp               # Check status
```

### Service Types

| Type | Description |
|------|------------|
| `simple` | Main process is the service (default) |
| `forking` | Service forks a child and parent exits |
| `oneshot` | Process exits after doing its task |
| `notify` | Service sends notification when ready |
| `idle` | Started after all jobs are done |

### Restart Policies

| Policy | Description |
|--------|------------|
| `no` | Never restart |
| `on-failure` | Restart on non-zero exit |
| `always` | Always restart |
| `on-abnormal` | Restart on signal/timeout |

---

## 4. System Targets (Runlevels)

```bash
# Check current target
systemctl get-default

# Set default target
sudo systemctl set-default multi-user.target    # No GUI
sudo systemctl set-default graphical.target     # With GUI

# Switch target now
sudo systemctl isolate multi-user.target
```

| Target | Old Runlevel | Description |
|--------|-------------|-------------|
| `poweroff.target` | 0 | Shut down |
| `rescue.target` | 1 | Single-user / rescue mode |
| `multi-user.target` | 3 | Multi-user, no GUI |
| `graphical.target` | 5 | Multi-user with GUI |
| `reboot.target` | 6 | Reboot |

---

## 5. System Power Commands

```bash
sudo systemctl poweroff              # Shut down
sudo systemctl reboot                # Reboot
sudo systemctl suspend               # Suspend (sleep)
sudo systemctl hibernate             # Hibernate

# Alternative shortcuts
sudo shutdown -h now                 # Shutdown now
sudo shutdown -r now                 # Reboot now
sudo shutdown -h +10                 # Shutdown in 10 minutes
sudo shutdown -c                     # Cancel scheduled shutdown
```

---

## 💡 Tips

- Always run `daemon-reload` after editing service files
- Use `journalctl -u service -f` when debugging service issues
- `Restart=on-failure` with `RestartSec=5` prevents restart loops
- Check `systemctl status` for the most recent log entries and errors
- Use `systemctl mask` to completely prevent a service from starting

---

[← Previous: Shell Scripting](01-shell-scripting.md) | [Back to Index](../README.md) | [Next: Cron & Scheduling →](03-cron-scheduling.md)
