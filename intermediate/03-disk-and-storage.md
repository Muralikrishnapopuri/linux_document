# 💾 Disk & Storage Management

Monitor disk usage, manage partitions, and work with mount points.

---

## 1. `df` — Disk Free (Filesystem Usage)

```bash
df                       # Show all mounted filesystems
df -h                    # Human-readable sizes (GB, MB)
df -h /                  # Usage of root partition
df -h /home              # Usage of /home partition
df -T                    # Show filesystem type
df -i                    # Show inode usage
```

### Reading the Output

```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   30G   18G  63% /
/dev/sda2       200G  120G   70G  63% /home
tmpfs            8G    0B    8G   0% /tmp
```

---

## 2. `du` — Disk Usage (Directory/File Size)

```bash
du file.txt              # Size of a file
du -h directory/         # Human-readable directory size
du -sh directory/        # Summary (total only)
du -sh *                 # Size of each item in current dir
du -sh */ | sort -rh     # Directories sorted by size
du -h --max-depth=1      # One level deep
du -ah                   # All files including hidden
```

### Practical Examples

```bash
# Find the 10 largest directories
du -sh */ 2>/dev/null | sort -rh | head -10

# Total size of current directory
du -sh .

# Find large files in a directory
du -ah /var | sort -rh | head -20
```

---

## 3. `lsblk` — List Block Devices

```bash
lsblk                   # List all block devices
lsblk -f                # Show filesystem and UUID
lsblk -o NAME,SIZE,FSTYPE,MOUNTPOINT  # Custom columns
```

---

## 4. `fdisk` — Partition Table Manager

```bash
# List all partitions
sudo fdisk -l

# Manage a specific disk (interactive)
sudo fdisk /dev/sda

# Common fdisk commands (inside the tool):
# p — Print partition table
# n — Create new partition
# d — Delete a partition
# w — Write changes and exit
# q — Quit without saving
```

---

## 5. `parted` — Advanced Partition Tool

```bash
sudo parted -l                    # List all partitions
sudo parted /dev/sda print       # Print partition table
```

---

## 6. `mount` / `umount` — Mount & Unmount Filesystems

### Mount

```bash
# Mount a USB drive
sudo mount /dev/sdb1 /mnt/usb

# Mount with specific filesystem type
sudo mount -t ext4 /dev/sdb1 /mnt/disk

# Mount an ISO file
sudo mount -o loop image.iso /mnt/iso

# Show all mounted filesystems
mount | column -t
```

### Unmount

```bash
sudo umount /mnt/usb             # Unmount by mount point
sudo umount /dev/sdb1             # Unmount by device
sudo umount -l /mnt/usb          # Lazy unmount (forces)
```

### Persistent Mounts — `/etc/fstab`

```bash
# View current fstab
cat /etc/fstab

# Add auto-mount entry (format):
# <device> <mount-point> <fs-type> <options> <dump> <pass>
# /dev/sdb1 /mnt/data ext4 defaults 0 2
```

---

## 7. `mkfs` — Create a Filesystem

```bash
# Format as ext4
sudo mkfs.ext4 /dev/sdb1

# Format as NTFS
sudo mkfs.ntfs /dev/sdb1

# Format as FAT32
sudo mkfs.vfat /dev/sdb1

# Format as XFS
sudo mkfs.xfs /dev/sdb1
```

> ⚠️ **WARNING**: This erases all data on the partition!

---

## 8. `fsck` — Filesystem Check & Repair

```bash
# Check a partition (must be unmounted)
sudo fsck /dev/sdb1

# Automatically fix errors
sudo fsck -y /dev/sdb1

# Check specific filesystem type
sudo fsck.ext4 /dev/sdb1
```

---

## 9. `blkid` — Block Device Attributes

```bash
sudo blkid                # Show UUID, type for all devices
sudo blkid /dev/sda1      # Info for specific partition
```

---

## 10. `dd` — Disk Duplicator

Low-level copy tool. Very powerful but dangerous.

```bash
# Create a bootable USB from ISO
sudo dd if=ubuntu.iso of=/dev/sdb bs=4M status=progress

# Backup entire disk
sudo dd if=/dev/sda of=/backup/disk.img bs=4M

# Create a 1GB test file
dd if=/dev/zero of=testfile bs=1M count=1024

# Wipe a disk
sudo dd if=/dev/zero of=/dev/sdb bs=4M status=progress
```

> ⚠️ **CAUTION**: `dd` with wrong `of=` can destroy your system. Triple-check device names!

---

## 11. `ncdu` — Visual Disk Usage Analyzer

```bash
# Install
sudo apt install ncdu

# Analyze current directory
ncdu

# Analyze specific directory
ncdu /var

# Navigate with arrow keys, press 'd' to delete
```

---

## 💡 Tips

- Use `df -h` for quick disk overview, `du -sh` for directory-specific sizes
- Always unmount before formatting or checking a filesystem
- Use `ncdu` for interactive disk usage exploration
- Check `lsblk` to identify the correct device before using `dd` or `mkfs`

---

[← Previous: Process Management](02-process-management.md) | [Back to Index](../README.md) | [Next: Networking →](04-networking.md)
