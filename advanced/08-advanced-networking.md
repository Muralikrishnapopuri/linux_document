# 🌐 Advanced Networking

Deep network diagnostics, traffic analysis, and security scanning.

---

## 1. `ss` — Socket Statistics (Replaces netstat)

```bash
ss                               # All connections
ss -t                            # TCP connections
ss -u                            # UDP connections
ss -l                            # Listening sockets
ss -tlnp                         # Listening TCP with process info
ss -ulnp                         # Listening UDP with process info
ss -s                            # Summary statistics
ss -o                            # Show timer info
ss state established             # Only established connections
ss -t dst 192.168.1.100          # Connections to specific IP
ss -t sport = :80                # Connections on port 80
```

### Common Combos

```bash
# What's listening on a port
ss -tlnp | grep :3000

# Count connections by state
ss -t | awk '{print $1}' | sort | uniq -c

# Find all connections to a host
ss -t dst 10.0.0.1
```

---

## 2. `nmap` — Network Scanner

```bash
# Install
sudo apt install nmap
```

### Host Discovery

```bash
nmap 192.168.1.1                 # Scan single host
nmap 192.168.1.0/24              # Scan entire subnet
nmap 192.168.1.1-50              # Scan IP range
nmap -sn 192.168.1.0/24          # Ping scan (host discovery)
```

### Port Scanning

```bash
nmap -sT 192.168.1.1             # TCP connect scan
nmap -sS 192.168.1.1             # SYN scan (stealth, needs root)
nmap -sU 192.168.1.1             # UDP scan
nmap -p 80,443 192.168.1.1      # Specific ports
nmap -p 1-1000 192.168.1.1      # Port range
nmap -p- 192.168.1.1             # All 65535 ports
nmap -F 192.168.1.1              # Fast scan (common ports)
```

### Service & OS Detection

```bash
nmap -sV 192.168.1.1             # Service version detection
nmap -O 192.168.1.1              # OS detection
nmap -A 192.168.1.1              # Aggressive (OS + version + scripts)
```

### Output Options

```bash
nmap -oN output.txt 192.168.1.1  # Normal output to file
nmap -oX output.xml 192.168.1.1  # XML output
nmap -oG output.gnmap 192.168.1.1 # Grepable output
```

> ⚠️ Only scan networks you own or have permission to scan!

---

## 3. `tcpdump` — Packet Capture

```bash
# Capture on interface
sudo tcpdump -i eth0

# Capture specific number of packets
sudo tcpdump -i eth0 -c 100

# Filter by host
sudo tcpdump -i eth0 host 192.168.1.1

# Filter by port
sudo tcpdump -i eth0 port 80
sudo tcpdump -i eth0 port 443

# Filter by protocol
sudo tcpdump -i eth0 tcp
sudo tcpdump -i eth0 udp
sudo tcpdump -i eth0 icmp

# Combine filters
sudo tcpdump -i eth0 'src 192.168.1.1 and dst port 80'

# Save to file
sudo tcpdump -i eth0 -w capture.pcap

# Read from file
tcpdump -r capture.pcap

# Show packet content (ASCII)
sudo tcpdump -i eth0 -A port 80

# Verbose output
sudo tcpdump -i eth0 -v
sudo tcpdump -i eth0 -vv                    # More verbose
```

---

## 4. `ip` — Advanced Network Configuration

### IP Addresses

```bash
ip addr show                     # Show all addresses
ip addr show eth0                # Specific interface
ip -4 addr                       # IPv4 only
ip -6 addr                       # IPv6 only

# Add/remove IP address
sudo ip addr add 192.168.1.200/24 dev eth0
sudo ip addr del 192.168.1.200/24 dev eth0
```

### Network Interfaces

```bash
ip link show                     # Show all interfaces
sudo ip link set eth0 up        # Enable interface
sudo ip link set eth0 down      # Disable interface
ip -s link                       # Interface statistics
```

### Routing

```bash
ip route show                    # Show routing table
ip route get 8.8.8.8            # Show route to specific IP

# Add/remove routes
sudo ip route add 10.0.0.0/24 via 192.168.1.1
sudo ip route del 10.0.0.0/24
sudo ip route add default via 192.168.1.1
```

### ARP Table

```bash
ip neigh show                    # Show ARP cache
ip neigh flush all               # Clear ARP cache
```

---

## 5. `iptables` — Advanced Firewall Rules

### Chains

| Chain | Purpose |
|-------|---------|
| `INPUT` | Incoming traffic to this machine |
| `OUTPUT` | Outgoing traffic from this machine |
| `FORWARD` | Traffic passing through (routing) |

### Common Rules

```bash
# View all rules
sudo iptables -L -v -n

# Allow established connections
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Allow SSH
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Allow HTTP/HTTPS
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT

# Block a specific IP
sudo iptables -A INPUT -s 10.0.0.5 -j DROP

# Rate limit (prevent brute force)
sudo iptables -A INPUT -p tcp --dport 22 -m limit --limit 3/min -j ACCEPT

# Log dropped packets
sudo iptables -A INPUT -j LOG --log-prefix "DROPPED: "

# Set default policy
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT

# NAT / Port forwarding
sudo iptables -t nat -A PREROUTING -p tcp --dport 8080 -j REDIRECT --to-port 80
```

---

## 6. `mtr` — Traceroute + Ping Combined

```bash
# Install
sudo apt install mtr

mtr google.com                   # Interactive mode
mtr -r google.com                # Report mode
mtr -r -c 50 google.com         # 50 packets report
mtr -n google.com                # No DNS resolution
```

---

## 7. `nc` (netcat) — Network Swiss Army Knife

```bash
# Test if port is open
nc -zv 192.168.1.1 80
nc -zv 192.168.1.1 20-100        # Port range

# Simple chat server
nc -l 1234                       # Listen on port 1234
nc 192.168.1.1 1234              # Connect from another machine

# File transfer
# Receiver:
nc -l 1234 > received_file.txt
# Sender:
nc 192.168.1.1 1234 < file.txt

# Port scanning
nc -zv 192.168.1.1 1-1000 2>&1 | grep succeeded

# Banner grabbing
echo "" | nc -w 3 192.168.1.1 80
```

---

## 8. `ethtool` — Network Interface Info

```bash
sudo ethtool eth0                # Interface details
sudo ethtool -i eth0             # Driver info
sudo ethtool -S eth0             # NIC statistics
sudo ethtool -s eth0 speed 1000  # Set speed
```

---

## 9. Network Troubleshooting Checklist

```bash
# 1. Check interface is up
ip link show

# 2. Check IP assignment
ip addr show

# 3. Check default gateway
ip route show

# 4. Check DNS resolution
nslookup google.com
dig google.com

# 5. Test connectivity
ping -c 3 8.8.8.8              # Test with IP
ping -c 3 google.com           # Test with DNS

# 6. Trace the route
traceroute google.com
mtr -r google.com

# 7. Check open ports
ss -tlnp

# 8. Check firewall
sudo iptables -L -n
sudo ufw status
```

---

## 💡 Tips

- Use `ss` instead of `netstat` — it's faster and more modern
- Use `mtr` for combined ping + traceroute analysis
- Always save `tcpdump` captures with `-w` for later analysis in Wireshark
- `nmap` scans should only be performed on networks you own
- Use `nc -zv` for quick port connectivity testing

---

[← Previous: Git](07-git-version-control.md) | [Back to Index](../README.md)
