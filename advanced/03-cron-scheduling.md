# ⏰ Cron Jobs & Task Scheduling

Automate recurring tasks with scheduled execution.

---

## 1. `crontab` — Cron Table

### Manage Your Crontab

```bash
crontab -e                  # Edit your crontab
crontab -l                  # List your cron jobs
crontab -r                  # Remove all your cron jobs
sudo crontab -u alice -e    # Edit alice's crontab
sudo crontab -u alice -l    # List alice's cron jobs
```

### Cron Time Format

```
* * * * *  command_to_run
│ │ │ │ │
│ │ │ │ └── Day of Week  (0-7, 0 and 7 = Sunday)
│ │ │ └──── Month        (1-12)
│ │ └────── Day of Month  (1-31)
│ └──────── Hour          (0-23)
└────────── Minute        (0-59)
```

### Common Schedules

| Schedule | Cron Expression | Description |
|----------|----------------|-------------|
| Every minute | `* * * * *` | Runs every minute |
| Every hour | `0 * * * *` | At minute 0 of every hour |
| Every day at midnight | `0 0 * * *` | At 00:00 every day |
| Every day at 6 AM | `0 6 * * *` | At 06:00 every day |
| Every Monday at 9 AM | `0 9 * * 1` | At 09:00 every Monday |
| Every 15 minutes | `*/15 * * * *` | Every 15 minutes |
| Every 6 hours | `0 */6 * * *` | At minute 0, every 6 hours |
| Weekdays at 8 AM | `0 8 * * 1-5` | Mon-Fri at 08:00 |
| First of month | `0 0 1 * *` | At midnight, 1st of month |
| Every Sunday at 2 AM | `0 2 * * 0` | At 02:00 every Sunday |

### Special Strings

| String | Equivalent | Description |
|--------|-----------|-------------|
| `@reboot` | — | Run once at startup |
| `@yearly` | `0 0 1 1 *` | Once a year |
| `@monthly` | `0 0 1 * *` | Once a month |
| `@weekly` | `0 0 * * 0` | Once a week |
| `@daily` | `0 0 * * *` | Once a day |
| `@hourly` | `0 * * * *` | Once an hour |

### Practical Examples

```bash
# Backup every night at 2 AM
0 2 * * * /home/user/scripts/backup.sh >> /var/log/backup.log 2>&1

# Clean temp files every Sunday at 3 AM
0 3 * * 0 find /tmp -mtime +7 -delete

# Run script at reboot
@reboot /home/user/scripts/startup.sh

# Health check every 5 minutes
*/5 * * * * curl -s http://localhost:8080/health >> /var/log/health.log

# Run on weekdays at 9 AM
0 9 * * 1-5 /home/user/scripts/morning_report.sh

# Multiple times per day
0 9,12,18 * * * /home/user/scripts/sync.sh
```

### Cron Output & Logging

```bash
# Redirect output to a log file
* * * * * /path/to/script.sh >> /var/log/myjob.log 2>&1

# Discard output
* * * * * /path/to/script.sh > /dev/null 2>&1

# Email output (requires mail configured)
MAILTO="user@example.com"
0 6 * * * /path/to/report.sh
```

---

## 2. System-Wide Cron

### /etc/crontab

```bash
sudo nano /etc/crontab
```

Format includes a user field:
```
* * * * * root /path/to/command
```

### Cron Directories

| Directory | Schedule |
|-----------|---------|
| `/etc/cron.d/` | Custom schedule files |
| `/etc/cron.hourly/` | Scripts run every hour |
| `/etc/cron.daily/` | Scripts run daily |
| `/etc/cron.weekly/` | Scripts run weekly |
| `/etc/cron.monthly/` | Scripts run monthly |

```bash
# Place a script in cron.daily to run daily
sudo cp myscript.sh /etc/cron.daily/
sudo chmod +x /etc/cron.daily/myscript.sh
```

---

## 3. `at` — One-Time Scheduled Tasks

Run a command once at a specific time.

```bash
# Install if needed
sudo apt install at

# Schedule a task
at 10:00 PM
> /home/user/scripts/cleanup.sh
> Ctrl+D  (to save)

# Schedule for a specific date
at 2:00 PM Dec 25

# Schedule relative to now
at now + 30 minutes
at now + 2 hours
at now + 1 day

# List pending jobs
atq

# Remove a job
atrm <job_number>

# View a job's details
at -c <job_number>
```

---

## 4. `anacron` — Cron for Non-24/7 Systems

Ensures tasks run even if the machine was off at the scheduled time.

```bash
# Config file
sudo nano /etc/anacrontab
```

### Format

```
# period  delay  job-identifier  command
1         5      daily-backup    /home/user/scripts/backup.sh
7         10     weekly-cleanup  /home/user/scripts/cleanup.sh
30        15     monthly-report  /home/user/scripts/report.sh
```

- **period**: Days between runs
- **delay**: Minutes to wait after boot before running
- **job-identifier**: Unique name

---

## 5. Systemd Timers (Modern Alternative)

### Create a Timer

**Service file** (`/etc/systemd/system/backup.service`):
```ini
[Unit]
Description=Daily Backup

[Service]
Type=oneshot
ExecStart=/home/user/scripts/backup.sh
```

**Timer file** (`/etc/systemd/system/backup.timer`):
```ini
[Unit]
Description=Run backup daily

[Timer]
OnCalendar=daily
Persistent=true

[Install]
WantedBy=timers.target
```

### Manage Timers

```bash
sudo systemctl daemon-reload
sudo systemctl enable backup.timer
sudo systemctl start backup.timer
systemctl list-timers                    # List all active timers
systemctl status backup.timer            # Timer status
```

---

## 💡 Tips

- Always redirect cron output to a log file for debugging
- Use full paths in cron jobs (cron has a minimal PATH)
- Test your script manually before adding it to cron
- Use `crontab.guru` website to validate cron expressions
- Prefer systemd timers for new installations

---

[← Previous: Systemd](02-systemd-services.md) | [Back to Index](../README.md) | [Next: Firewall & Security →](04-firewall-security.md)
