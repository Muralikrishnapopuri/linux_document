# 🌍 Common Real-World Scenarios

Step-by-step guides for tasks you'll encounter in everyday Linux use.

---

## 1. Setting Up a Web Server (Nginx)

```bash
# Install Nginx
sudo apt update && sudo apt install nginx -y

# Start and enable
sudo systemctl start nginx
sudo systemctl enable nginx

# Check status
sudo systemctl status nginx

# Open firewall
sudo ufw allow 'Nginx Full'

# Test — visit http://localhost in browser

# Edit default site
sudo nano /etc/nginx/sites-available/default

# Test config and reload
sudo nginx -t
sudo systemctl reload nginx

# View access logs
sudo tail -f /var/log/nginx/access.log
```

---

## 2. Setting Up a New User & Environment

```bash
# Create user with home directory
sudo useradd -m -s /bin/bash newuser
sudo passwd newuser

# Add to sudo group
sudo usermod -aG sudo newuser

# Switch to new user
su - newuser

# Set up SSH access for the user
sudo mkdir -p /home/newuser/.ssh
sudo cp ~/.ssh/authorized_keys /home/newuser/.ssh/
sudo chown -R newuser:newuser /home/newuser/.ssh
sudo chmod 700 /home/newuser/.ssh
sudo chmod 600 /home/newuser/.ssh/authorized_keys
```

---

## 3. Automated Backups with Cron

```bash
# Create backup script
cat > ~/backup.sh << 'EOF'
#!/bin/bash
DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_DIR="/backup"
SOURCE="/home/murali-krishna/projects"

mkdir -p "$BACKUP_DIR"
tar -czf "$BACKUP_DIR/projects_$DATE.tar.gz" "$SOURCE"

# Keep only last 7 backups
ls -t "$BACKUP_DIR"/projects_*.tar.gz | tail -n +8 | xargs rm -f
echo "Backup completed: $DATE"
EOF

chmod +x ~/backup.sh

# Schedule daily backup at 2 AM
crontab -e
# Add this line:
# 0 2 * * * /home/murali-krishna/backup.sh >> /var/log/backup.log 2>&1

# Verify cron job
crontab -l
```

---

## 4. Deploying a Node.js Application

```bash
# Install Node.js (via NodeSource)
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install nodejs -y

# Verify
node -v && npm -v

# Clone and set up project
git clone https://github.com/user/project.git
cd project
npm install

# Create systemd service
sudo tee /etc/systemd/system/myapp.service << EOF
[Unit]
Description=My Node.js App
After=network.target

[Service]
Type=simple
User=murali-krishna
WorkingDirectory=/home/murali-krishna/project
ExecStart=/usr/bin/node app.js
Restart=on-failure
Environment=NODE_ENV=production PORT=3000

[Install]
WantedBy=multi-user.target
EOF

# Start and enable
sudo systemctl daemon-reload
sudo systemctl start myapp
sudo systemctl enable myapp
sudo systemctl status myapp
```

---

## 5. Docker Workflow

```bash
# Install Docker
sudo apt update
sudo apt install docker.io docker-compose -y
sudo usermod -aG docker $USER
# Log out and back in for group changes

# Build and run a container
docker build -t myapp .
docker run -d -p 8080:80 --name myapp myapp

# Common Docker commands
docker ps                        # Running containers
docker ps -a                     # All containers
docker logs myapp                # View logs
docker exec -it myapp bash       # Shell into container
docker stop myapp                # Stop container
docker rm myapp                  # Remove container

# Docker Compose workflow
docker-compose up -d             # Start services
docker-compose logs -f           # Follow logs
docker-compose down              # Stop and remove
docker-compose restart           # Restart services

# Cleanup
docker system prune -a           # Remove unused everything
docker volume prune              # Remove unused volumes
```

---

## 6. Setting Up SSL with Let's Encrypt

```bash
# Install Certbot
sudo apt install certbot python3-certbot-nginx -y

# Get certificate (replace domain)
sudo certbot --nginx -d example.com -d www.example.com

# Test auto-renewal
sudo certbot renew --dry-run

# Auto-renewal is set up automatically via systemd timer
sudo systemctl status certbot.timer
```

---

## 7. Database Management (MySQL/PostgreSQL)

### MySQL

```bash
# Install
sudo apt install mysql-server -y
sudo mysql_secure_installation

# Access MySQL
sudo mysql -u root -p

# Common operations
mysql> CREATE DATABASE myapp;
mysql> CREATE USER 'appuser'@'localhost' IDENTIFIED BY 'password';
mysql> GRANT ALL ON myapp.* TO 'appuser'@'localhost';
mysql> FLUSH PRIVILEGES;

# Backup database
mysqldump -u root -p myapp > backup.sql

# Restore database
mysql -u root -p myapp < backup.sql
```

### PostgreSQL

```bash
# Install
sudo apt install postgresql postgresql-contrib -y

# Access PostgreSQL
sudo -u postgres psql

# Common operations
postgres=# CREATE DATABASE myapp;
postgres=# CREATE USER appuser WITH PASSWORD 'password';
postgres=# GRANT ALL PRIVILEGES ON DATABASE myapp TO appuser;

# Backup
pg_dump myapp > backup.sql

# Restore
psql myapp < backup.sql
```

---

## 8. Log Rotation Setup

```bash
# Create custom logrotate config
sudo tee /etc/logrotate.d/myapp << EOF
/var/log/myapp/*.log {
    daily
    missingok
    rotate 14
    compress
    delaycompress
    notifempty
    create 0640 www-data www-data
    sharedscripts
    postrotate
        systemctl reload myapp > /dev/null 2>&1 || true
    endscript
}
EOF

# Test the config
sudo logrotate -d /etc/logrotate.d/myapp

# Force rotation
sudo logrotate -f /etc/logrotate.d/myapp
```

---

## 9. System Hardening Checklist

```bash
# 1. Update system
sudo apt update && sudo apt upgrade -y

# 2. Enable firewall
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
sudo ufw enable

# 3. Disable root SSH login
sudo sed -i 's/PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
sudo systemctl restart sshd

# 4. Set up fail2ban
sudo apt install fail2ban -y
sudo systemctl enable fail2ban

# 5. Enable automatic security updates
sudo apt install unattended-upgrades -y
sudo dpkg-reconfigure -plow unattended-upgrades

# 6. Check for open ports
sudo ss -tlnp

# 7. Review running services
systemctl list-units --type=service --state=running
```

---

## 💡 Tips

- Always test config changes before applying: `nginx -t`, `apachectl configtest`
- Use `systemctl` to manage services — learn `start`, `stop`, `restart`, `status`, `enable`
- Set up log rotation early — unmanaged logs can fill your disk
- Use environment variables for secrets, never hardcode passwords in scripts
- Automate repetitive tasks with cron and shell scripts

---

[← Back to Index](../README.md)
