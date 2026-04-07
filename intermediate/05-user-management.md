# 👥 User Management

Create, modify, and manage users and groups.

---

## 1. `useradd` — Create a New User

```bash
sudo useradd alice                           # Create user (basic)
sudo useradd -m alice                        # Create with home directory
sudo useradd -m -s /bin/bash alice           # With home dir and bash shell
sudo useradd -m -G sudo,developers alice     # With groups
sudo useradd -m -e 2024-12-31 tempuser      # With expiration date
```

### Common Options

| Option | Description |
|--------|------------|
| `-m` | Create home directory |
| `-s /bin/bash` | Set login shell |
| `-G group1,group2` | Add to supplementary groups |
| `-d /custom/home` | Set custom home directory |
| `-e YYYY-MM-DD` | Account expiration date |
| `-c "Full Name"` | Comment (usually full name) |

---

## 2. `adduser` — Friendly User Creation (Ubuntu/Debian)

```bash
sudo adduser alice                # Interactive — prompts for password, name, etc.
```

> `adduser` is a user-friendly wrapper around `useradd`. Preferred on Debian/Ubuntu.

---

## 3. `passwd` — Set or Change Password

```bash
passwd                           # Change your own password
sudo passwd alice                # Change alice's password
sudo passwd -l alice             # Lock account (disable login)
sudo passwd -u alice             # Unlock account
sudo passwd -e alice             # Force password change at next login
sudo passwd -S alice             # Show password status
```

---

## 4. `usermod` — Modify a User

```bash
# Change username
sudo usermod -l new_name old_name

# Change home directory
sudo usermod -d /new/home -m alice

# Change shell
sudo usermod -s /bin/zsh alice

# Add to a group (append)
sudo usermod -aG sudo alice

# Add to multiple groups
sudo usermod -aG docker,developers alice

# Lock/unlock account
sudo usermod -L alice              # Lock
sudo usermod -U alice              # Unlock

# Set expiration date
sudo usermod -e 2024-12-31 alice
```

> ⚠️ Always use `-aG` (append) when adding groups. Without `-a`, existing groups are replaced!

---

## 5. `userdel` — Delete a User

```bash
sudo userdel alice               # Delete user (keep home dir)
sudo userdel -r alice            # Delete user AND home directory
```

---

## 6. `groupadd` — Create a Group

```bash
sudo groupadd developers
sudo groupadd -g 1500 custom_gid    # With specific GID
```

---

## 7. `groupmod` — Modify a Group

```bash
sudo groupmod -n new_name old_name   # Rename group
sudo groupmod -g 2000 groupname      # Change GID
```

---

## 8. `groupdel` — Delete a Group

```bash
sudo groupdel developers
```

---

## 9. `groups` — Show User's Groups

```bash
groups                     # Groups of current user
groups alice               # Groups of specific user
```

---

## 10. `id` — User and Group IDs

```bash
id                         # Current user info
id alice                   # Specific user info
id -u alice                # User ID only
id -g alice                # Primary group ID only
id -Gn alice               # All group names
```

---

## 11. Viewing User Information

```bash
# List all users
cat /etc/passwd

# List all groups
cat /etc/group

# Currently logged-in users
who
w
users

# Last login info
last
last alice
lastlog
```

### Understanding `/etc/passwd`

```
alice:x:1001:1001:Alice Smith:/home/alice:/bin/bash
  │   │  │    │      │            │          │
  │   │  │    │      │            │          └── Shell
  │   │  │    │      │            └───────────── Home directory
  │   │  │    │      └────────────────────────── Comment (full name)
  │   │  │    └───────────────────────────────── Primary GID
  │   │  └─────────────────────────────────────── UID
  │   └─────────────────────────────────────────── Password (x = in /etc/shadow)
  └──────────────────────────────────────────────── Username
```

---

## 12. `su` / `sudo` — Switch User and Elevated Privileges

```bash
# Switch to another user
su alice                   # Switch to alice (need alice's password)
su - alice                 # Switch with full login environment
su -                       # Switch to root

# Run command as root
sudo command               # Run single command as root
sudo -u alice command      # Run command as alice
sudo -i                    # Interactive root shell
sudo -s                    # Root shell (keeps current env)
sudo !!                    # Re-run last command with sudo
```

### Manage Sudoers

```bash
sudo visudo                # Edit sudoers file safely

# Grant sudo access to a user (add this line in visudo):
# alice ALL=(ALL:ALL) ALL

# Or simply add user to sudo group:
sudo usermod -aG sudo alice
```

---

## 13. `chsh` — Change Shell

```bash
chsh                       # Change your own shell
chsh -s /bin/zsh           # Set to zsh
chsh -s /bin/bash alice    # Change alice's shell

# List available shells
cat /etc/shells
```

---

## 💡 Tips

- Always use `adduser` (not `useradd`) on Ubuntu for interactive creation
- Remember `-aG` when adding groups — `-G` alone replaces all groups
- Use `sudo visudo` to edit sudoers — never edit the file directly
- Check `who` to see who's currently logged in
- Use `sudo -i` for a full root shell session

---

[← Previous: Networking](04-networking.md) | [Back to Index](../README.md) | [Next: Package Management →](06-package-management.md)
