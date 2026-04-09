# 🐧 Linux Commands - Complete Step-by-Step Documentation

A comprehensive guide to Linux commands organized by difficulty level. Each file contains commands with syntax, options, practical examples, and tips.

---

## 📁 Directory Structure

```
Linux/
├── README.md                  ← You are here
├── basic/                     ← Start here if you're new
│   ├── 01-file-system-navigation.md
│   ├── 02-file-operations.md
│   ├── 03-file-viewing.md
│   ├── 04-file-permissions.md
│   ├── 05-search-and-find.md
│   ├── 06-system-info.md
│   └── 07-help-and-manual.md
├── intermediate/
│   ├── 01-text-processing.md
│   ├── 02-process-management.md
│   ├── 03-disk-and-storage.md
│   ├── 04-networking.md
│   ├── 05-user-management.md
│   ├── 06-package-management.md
│   ├── 07-archiving-compression.md
│   └── 08-io-redirection-piping.md
├── advanced/
│   ├── 01-shell-scripting.md
│   ├── 02-systemd-services.md
│   ├── 03-cron-scheduling.md
│   ├── 04-firewall-security.md
│   ├── 05-performance-monitoring.md
│   ├── 06-docker-basics.md
│   ├── 07-git-version-control.md
│   └── 08-advanced-networking.md
└── practical-terminal-guidance/  ← Hands-on recipes & workflows
    ├── 01-terminal-basics-and-shortcuts.md
    ├── 02-command-chaining-workflows.md
    ├── 03-file-directory-management.md
    ├── 04-text-processing-recipes.md
    ├── 05-system-troubleshooting.md
    ├── 06-productivity-hacks.md
    └── 07-real-world-scenarios.md
```

---

## 🟢 Basic (Start Here)

| # | Topic | Key Commands |
|---|-------|-------------|
| 1 | [File System Navigation](basic/01-file-system-navigation.md) | `cd`, `ls`, `pwd`, `tree` |
| 2 | [File Operations](basic/02-file-operations.md) | `cp`, `mv`, `rm`, `touch`, `mkdir` |
| 3 | [File Viewing](basic/03-file-viewing.md) | `cat`, `less`, `head`, `tail`, `wc` |
| 4 | [File Permissions](basic/04-file-permissions.md) | `chmod`, `chown`, `chgrp` |
| 5 | [Search & Find](basic/05-search-and-find.md) | `find`, `locate`, `grep`, `which` |
| 6 | [System Information](basic/06-system-info.md) | `uname`, `hostname`, `uptime`, `whoami` |
| 7 | [Help & Manual](basic/07-help-and-manual.md) | `man`, `help`, `info`, `whatis` |

## 🟡 Intermediate

| # | Topic | Key Commands |
|---|-------|-------------|
| 1 | [Text Processing](intermediate/01-text-processing.md) | `sed`, `awk`, `cut`, `sort`, `uniq`, `tr` |
| 2 | [Process Management](intermediate/02-process-management.md) | `ps`, `top`, `kill`, `bg`, `fg`, `jobs` |
| 3 | [Disk & Storage](intermediate/03-disk-and-storage.md) | `df`, `du`, `mount`, `fdisk`, `lsblk` |
| 4 | [Networking](intermediate/04-networking.md) | `ping`, `curl`, `wget`, `ssh`, `scp` |
| 5 | [User Management](intermediate/05-user-management.md) | `useradd`, `usermod`, `passwd`, `groups` |
| 6 | [Package Management](intermediate/06-package-management.md) | `apt`, `dpkg`, `snap` |
| 7 | [Archiving & Compression](intermediate/07-archiving-compression.md) | `tar`, `gzip`, `zip`, `unzip` |
| 8 | [I/O Redirection & Piping](intermediate/08-io-redirection-piping.md) | `>`, `>>`, `\|`, `tee`, `xargs` |

## 🔴 Advanced

| # | Topic | Key Commands |
|---|-------|-------------|
| 1 | [Shell Scripting](advanced/01-shell-scripting.md) | variables, loops, functions |
| 2 | [Systemd & Services](advanced/02-systemd-services.md) | `systemctl`, `journalctl` |
| 3 | [Cron & Scheduling](advanced/03-cron-scheduling.md) | `crontab`, `at`, `anacron` |
| 4 | [Firewall & Security](advanced/04-firewall-security.md) | `ufw`, `iptables`, `fail2ban` |
| 5 | [Performance Monitoring](advanced/05-performance-monitoring.md) | `htop`, `vmstat`, `iostat`, `strace` |
| 6 | [Docker Basics](advanced/06-docker-basics.md) | `docker run`, `docker build` |
| 7 | [Git Version Control](advanced/07-git-version-control.md) | `git add`, `git commit`, `git push` |
| 8 | [Advanced Networking](advanced/08-advanced-networking.md) | `ss`, `nmap`, `tcpdump` |

## 🧪 Practical Terminal Guidance

| # | Topic | What You'll Learn |
|---|-------|-------------------|
| 1 | [Terminal Basics & Shortcuts](practical-terminal-guidance/01-terminal-basics-and-shortcuts.md) | Keyboard shortcuts, history, tab completion, tmux |
| 2 | [Command Chaining & Workflows](practical-terminal-guidance/02-command-chaining-workflows.md) | Pipes, redirection, xargs, process substitution |
| 3 | [File & Directory Management](practical-terminal-guidance/03-file-directory-management.md) | Bulk rename, find recipes, permissions, links |
| 4 | [Text Processing Recipes](practical-terminal-guidance/04-text-processing-recipes.md) | grep, sed, awk, cut, sort, uniq recipes |
| 5 | [System Troubleshooting](practical-terminal-guidance/05-system-troubleshooting.md) | Boot, disk, CPU, memory, network diagnostics |
| 6 | [Productivity Hacks](practical-terminal-guidance/06-productivity-hacks.md) | Aliases, functions, SSH config, dotfiles |
| 7 | [Real-World Scenarios](practical-terminal-guidance/07-real-world-scenarios.md) | Server setup, Docker, databases, deployments |

---

## ⚡ Quick Tips

- Press `Tab` to auto-complete commands and file names
- Use `↑` / `↓` arrows to navigate command history
- `Ctrl+C` cancels a running command
- `Ctrl+L` clears the terminal screen
- `Ctrl+R` searches your command history
- Most commands support `--help` flag for quick usage info
