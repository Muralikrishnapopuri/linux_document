# 📦 Package Management

Install, update, and remove software on Ubuntu/Debian.

---

## 1. `apt` — Advanced Package Tool (Primary)

### Update & Upgrade

```bash
sudo apt update                      # Refresh package list
sudo apt upgrade                     # Upgrade all installed packages
sudo apt full-upgrade                # Upgrade with dependency handling
sudo apt update && sudo apt upgrade  # Common combo
```

### Install Packages

```bash
sudo apt install nginx               # Install a package
sudo apt install git vim curl        # Install multiple packages
sudo apt install -y package_name     # Auto-confirm (yes to all)
sudo apt install ./package.deb       # Install local .deb file
```

### Remove Packages

```bash
sudo apt remove nginx                # Remove package (keep config)
sudo apt purge nginx                 # Remove package + config files
sudo apt autoremove                  # Remove unused dependencies
sudo apt clean                       # Clear downloaded package cache
sudo apt autoclean                   # Remove old cached packages
```

### Search & Info

```bash
apt search nginx                     # Search for packages
apt show nginx                       # Show package details
apt list --installed                 # List installed packages
apt list --upgradable                # List upgradable packages
apt depends nginx                    # Show dependencies
```

---

## 2. `dpkg` — Debian Package Manager (Low-Level)

```bash
# Install a .deb file
sudo dpkg -i package.deb

# Remove a package
sudo dpkg -r package_name

# Purge (remove + config)
sudo dpkg -P package_name

# List installed packages
dpkg -l
dpkg -l | grep nginx

# Check if a package is installed
dpkg -s nginx

# List files installed by a package
dpkg -L nginx

# Find which package owns a file
dpkg -S /usr/bin/vim

# Fix broken dependencies after dpkg
sudo apt install -f
```

---

## 3. `snap` — Snap Package Manager

```bash
# Install a snap
sudo snap install vlc

# Install from edge channel
sudo snap install app --edge

# List installed snaps
snap list

# Update snaps
sudo snap refresh

# Remove a snap
sudo snap remove vlc

# Find snaps
snap find "media player"

# Show snap info
snap info vlc
```

---

## 4. `flatpak` — Flatpak Package Manager

```bash
# Install flatpak (if not present)
sudo apt install flatpak

# Add Flathub repository
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

# Install an app
flatpak install flathub com.spotify.Client

# Run a flatpak app
flatpak run com.spotify.Client

# List installed flatpaks
flatpak list

# Update all flatpaks
flatpak update

# Remove a flatpak
flatpak uninstall com.spotify.Client
```

---

## 5. Managing PPAs (Personal Package Archives)

```bash
# Add a PPA
sudo add-apt-repository ppa:user/ppa-name
sudo apt update

# Remove a PPA
sudo add-apt-repository --remove ppa:user/ppa-name

# List all PPAs
grep -r "^deb" /etc/apt/sources.list.d/
```

---

## 6. Package Management Quick Reference

| Task | Command |
|------|---------|
| Update package list | `sudo apt update` |
| Upgrade packages | `sudo apt upgrade` |
| Install package | `sudo apt install pkg` |
| Remove package | `sudo apt remove pkg` |
| Remove + config | `sudo apt purge pkg` |
| Search packages | `apt search keyword` |
| Package info | `apt show pkg` |
| List installed | `apt list --installed` |
| Clean cache | `sudo apt clean` |
| Remove unused | `sudo apt autoremove` |
| Install .deb file | `sudo dpkg -i file.deb` |
| Fix broken deps | `sudo apt install -f` |

---

## 💡 Tips

- Always run `sudo apt update` before installing new packages
- Use `apt` instead of `apt-get` — it has a friendlier interface
- `sudo apt autoremove` regularly to free up disk space
- Snap packages are sandboxed and auto-update
- Use `dpkg -S /path/to/file` to find which package installed a specific file

---

[← Previous: User Management](05-user-management.md) | [Back to Index](../README.md) | [Next: Archiving & Compression →](07-archiving-compression.md)
