# 🌐 Networking

Network diagnostics, file transfers, and remote access.

---

## 1. `ping` — Test Network Connectivity

```bash
ping google.com                  # Continuous ping
ping -c 5 google.com             # Send 5 packets then stop
ping -i 2 google.com             # Ping every 2 seconds
ping -W 3 192.168.1.1            # Timeout after 3 seconds
ping -s 1000 google.com          # Custom packet size (bytes)
```

---

## 2. `curl` — Transfer Data via URLs

### Basic Requests

```bash
curl https://example.com                    # GET request
curl -o file.html https://example.com       # Save to file
curl -O https://example.com/file.zip        # Save with original name
curl -I https://example.com                 # Headers only
curl -L https://example.com                 # Follow redirects
```

### POST Requests

```bash
# Send form data
curl -X POST -d "name=john&age=30" https://api.example.com/users

# Send JSON data
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{"name":"john","age":30}' \
  https://api.example.com/users
```

### Other Methods

```bash
curl -X PUT -d '{"name":"jane"}' https://api.example.com/users/1
curl -X DELETE https://api.example.com/users/1
curl -X PATCH -d '{"age":31}' https://api.example.com/users/1
```

### Useful Options

| Option | Description |
|--------|------------|
| `-v` | Verbose output |
| `-s` | Silent mode (no progress) |
| `-u user:pass` | Basic authentication |
| `-H "Header: Value"` | Custom header |
| `-k` | Ignore SSL certificate errors |
| `--max-time 10` | Timeout in seconds |

---

## 3. `wget` — Download Files

```bash
wget https://example.com/file.zip                  # Download file
wget -O custom_name.zip https://example.com/file    # Custom filename
wget -c https://example.com/large.iso               # Resume download
wget -q https://example.com/file.zip                # Quiet mode
wget -r -l 2 https://example.com                    # Recursive (2 levels deep)
wget -b https://example.com/large.iso               # Download in background
wget --limit-rate=500k https://example.com/file     # Limit speed
wget -i urls.txt                                     # Download from URL list
```

---

## 4. `ssh` — Secure Shell (Remote Access)

```bash
ssh user@192.168.1.100               # Connect to remote server
ssh -p 2222 user@server.com          # Custom port
ssh user@server.com "ls -la"         # Run a single command remotely
ssh -i ~/.ssh/id_rsa user@server     # Specify key file
```

### SSH Key Setup

```bash
# Generate SSH key pair
ssh-keygen -t ed25519 -C "your@email.com"

# Copy public key to server (passwordless login)
ssh-copy-id user@server.com

# SSH config file (~/.ssh/config)
# Host myserver
#     HostName 192.168.1.100
#     User alice
#     Port 22
#     IdentityFile ~/.ssh/id_rsa

# Then connect with:
ssh myserver
```

---

## 5. `scp` — Secure Copy (File Transfer over SSH)

```bash
# Copy file TO remote server
scp file.txt user@server:/home/user/

# Copy file FROM remote server
scp user@server:/home/user/file.txt ./

# Copy directory recursively
scp -r project/ user@server:/home/user/

# Custom port
scp -P 2222 file.txt user@server:/home/user/
```

---

## 6. `rsync` — Advanced File Sync

More efficient than `scp` — syncs only changes.

```bash
# Sync local directory to remote
rsync -avz source/ user@server:/backup/

# Sync remote to local
rsync -avz user@server:/data/ ./local_data/

# Dry run (preview changes)
rsync -avzn source/ destination/

# Delete files in destination not in source
rsync -avz --delete source/ destination/

# Show progress
rsync -avz --progress source/ destination/
```

### Common Options

| Option | Description |
|--------|------------|
| `-a` | Archive mode (preserves everything) |
| `-v` | Verbose |
| `-z` | Compress during transfer |
| `-n` | Dry run |
| `--delete` | Delete extraneous files from destination |
| `--exclude` | Exclude files/patterns |
| `--progress` | Show transfer progress |

---

## 7. `ip` — Network Interface Configuration

```bash
ip addr                              # Show all IP addresses
ip addr show eth0                     # Show specific interface
ip link                               # Show network interfaces
ip route                              # Show routing table
ip route | grep default               # Show default gateway
ip -s link                            # Show network statistics
```

---

## 8. `ifconfig` — Legacy Network Config

```bash
ifconfig                              # Show all interfaces
ifconfig eth0                         # Show specific interface
sudo ifconfig eth0 up                 # Enable interface
sudo ifconfig eth0 down               # Disable interface
```

> Note: `ifconfig` is deprecated. Use `ip` instead.

---

## 9. `netstat` — Network Statistics

```bash
netstat -tlnp                         # Listening TCP ports
netstat -ulnp                         # Listening UDP ports
netstat -an                           # All connections
netstat -r                            # Routing table
```

> Note: `netstat` is deprecated. Use `ss` instead (covered in advanced).

---

## 10. `traceroute` — Trace Network Path

```bash
traceroute google.com                 # Trace route to host
traceroute -n google.com              # Skip DNS resolution
```

---

## 11. `nslookup` / `dig` — DNS Lookup

```bash
# nslookup
nslookup google.com
nslookup -type=MX gmail.com          # Mail server records

# dig (more detailed)
dig google.com
dig google.com A                     # A records
dig google.com MX                    # Mail records
dig +short google.com                # Short output
```

---

## 12. `host` — Simple DNS Lookup

```bash
host google.com
host -t MX gmail.com
```

---

## 13. `hostname` — Network Identity

```bash
hostname                  # Show hostname
hostname -I               # Show all IP addresses
hostname -f               # Fully qualified domain name
```

---

## 💡 Tips

- Use `rsync` instead of `scp` for large or recurring transfers
- Set up SSH keys for passwordless login
- Use SSH config file (`~/.ssh/config`) to save connection details
- `curl -s` is useful in scripts; `wget` is better for downloading files
- Always check `ip addr` to find your machine's IP address

---

[← Previous: Disk & Storage](03-disk-and-storage.md) | [Back to Index](../README.md) | [Next: User Management →](05-user-management.md)
