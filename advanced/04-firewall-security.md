# 🛡️ Firewall & Security

Protect your system with firewall rules and security tools.

---

## 1. `ufw` — Uncomplicated Firewall

The easiest firewall for Ubuntu/Debian.

### Enable/Disable

```bash
sudo ufw enable                      # Enable firewall
sudo ufw disable                     # Disable firewall
sudo ufw status                      # Check status
sudo ufw status verbose              # Detailed status
sudo ufw status numbered             # Show rules with numbers
```

### Allow/Deny Rules

```bash
# Allow by port
sudo ufw allow 22                    # Allow SSH
sudo ufw allow 80                    # Allow HTTP
sudo ufw allow 443                   # Allow HTTPS
sudo ufw allow 3000                  # Allow custom port

# Allow by service name
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https

# Allow port range
sudo ufw allow 6000:6010/tcp

# Allow from specific IP
sudo ufw allow from 192.168.1.100
sudo ufw allow from 192.168.1.100 to any port 22

# Allow subnet
sudo ufw allow from 192.168.1.0/24

# Deny rules
sudo ufw deny 23                     # Deny telnet
sudo ufw deny from 10.0.0.5          # Block specific IP
```

### Delete Rules

```bash
sudo ufw delete allow 80             # Delete by rule
sudo ufw status numbered             # Show numbered rules
sudo ufw delete 3                    # Delete rule #3
```

### Default Policies

```bash
sudo ufw default deny incoming       # Block all incoming (recommended)
sudo ufw default allow outgoing      # Allow all outgoing
```

### Common Server Setup

```bash
# Typical web server firewall
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https
sudo ufw enable
```

---

## 2. `iptables` — Advanced Firewall

Lower-level firewall tool with full control.

### View Rules

```bash
sudo iptables -L                     # List all rules
sudo iptables -L -n -v               # Verbose with numbers
sudo iptables -L --line-numbers      # With line numbers
```

### Basic Rules

```bash
# Allow incoming SSH
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Allow incoming HTTP
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT

# Allow established connections
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Block an IP
sudo iptables -A INPUT -s 10.0.0.5 -j DROP

# Block all incoming (default deny)
sudo iptables -P INPUT DROP

# Allow all outgoing
sudo iptables -P OUTPUT ACCEPT

# Allow loopback
sudo iptables -A INPUT -i lo -j ACCEPT
```

### Delete Rules

```bash
sudo iptables -D INPUT 3             # Delete rule #3
sudo iptables -F                     # Flush (delete) all rules
```

### Save/Restore Rules

```bash
# Save rules
sudo iptables-save > /etc/iptables/rules.v4

# Restore rules
sudo iptables-restore < /etc/iptables/rules.v4

# Install persistent iptables
sudo apt install iptables-persistent
sudo netfilter-persistent save
```

---

## 3. `fail2ban` — Intrusion Prevention

Automatically bans IPs with too many failed login attempts.

### Install and Start

```bash
sudo apt install fail2ban
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
```

### Configuration

```bash
# Create local config (don't edit jail.conf directly)
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo nano /etc/fail2ban/jail.local
```

### Basic Configuration

```ini
[DEFAULT]
bantime  = 3600       # Ban for 1 hour (seconds)
findtime = 600        # Within 10 minutes
maxretry = 5          # After 5 failures
banaction = ufw       # Use ufw for banning

[sshd]
enabled = true
port = ssh
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
```

### Management Commands

```bash
sudo fail2ban-client status                    # Overall status
sudo fail2ban-client status sshd               # SSH jail status
sudo fail2ban-client set sshd unbanip 1.2.3.4  # Unban an IP
sudo fail2ban-client set sshd banip 1.2.3.4    # Manually ban IP
```

---

## 4. SSH Security Hardening

### Edit SSH Config

```bash
sudo nano /etc/ssh/sshd_config
```

### Recommended Settings

```bash
# Disable root login
PermitRootLogin no

# Disable password authentication (use keys only)
PasswordAuthentication no

# Change default port
Port 2222

# Allow specific users only
AllowUsers alice bob

# Limit authentication attempts
MaxAuthTries 3

# Disable empty passwords
PermitEmptyPasswords no

# Set idle timeout
ClientAliveInterval 300
ClientAliveCountMax 2
```

### Apply Changes

```bash
sudo systemctl restart sshd
```

---

## 5. File Integrity & Auditing

### Check for SUID/SGID Files

```bash
# Find all SUID files
sudo find / -perm -4000 -type f 2>/dev/null

# Find all SGID files
sudo find / -perm -2000 -type f 2>/dev/null

# Find world-writable files
sudo find / -perm -o+w -type f 2>/dev/null
```

### Check Open Ports

```bash
sudo ss -tlnp                       # Listening TCP ports
sudo ss -ulnp                       # Listening UDP ports
sudo netstat -tlnp                  # Alternative
```

### Check Login History

```bash
last                                # Recent logins
lastb                               # Failed login attempts
who                                 # Currently logged in
w                                   # Who + what they're doing
```

---

## 6. Automatic Security Updates

```bash
# Install unattended-upgrades
sudo apt install unattended-upgrades

# Enable
sudo dpkg-reconfigure unattended-upgrades

# Check config
cat /etc/apt/apt.conf.d/50unattended-upgrades
```

---

## 💡 Tips

- Always enable `ufw` on internet-facing servers
- Use SSH keys instead of passwords for remote access
- Change the default SSH port from 22 to reduce automated attacks
- Set up `fail2ban` on any server with SSH access
- Regularly check `last` and `lastb` for suspicious logins

---

[← Previous: Cron](03-cron-scheduling.md) | [Back to Index](../README.md) | [Next: Performance Monitoring →](05-performance-monitoring.md)
