# ⚡ Productivity Hacks & Time Savers

Speed up your daily workflow with these terminal productivity tricks.

---

## 1. Aliases — Custom Shortcuts

### Create Temporary Aliases

```bash
alias ll='ls -la'
alias gs='git status'
alias update='sudo apt update && sudo apt upgrade -y'
alias ..='cd ..'
alias ...='cd ../..'
```

### Make Aliases Permanent

Add to `~/.bashrc`:

```bash
# --- Navigation ---
alias ll='ls -la --color=auto'
alias ..='cd ..'
alias ...='cd ../..'

# --- Safety ---
alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'

# --- Git ---
alias gs='git status'
alias ga='git add'
alias gc='git commit -m'
alias gp='git push'
alias gl='git log --oneline -20'

# --- System ---
alias update='sudo apt update && sudo apt upgrade -y'
alias ports='sudo ss -tlnp'
alias myip='curl -s ifconfig.me'
alias meminfo='free -h'

# Apply changes
source ~/.bashrc
```

---

## 2. Shell Functions

Add to `~/.bashrc`:

```bash
# Create a directory and cd into it
mkcd() { mkdir -p "$1" && cd "$1"; }

# Extract any archive
extract() {
    case "$1" in
        *.tar.bz2) tar xjf "$1"  ;;
        *.tar.gz)  tar xzf "$1"  ;;
        *.tar.xz)  tar xJf "$1"  ;;
        *.bz2)     bunzip2 "$1"  ;;
        *.gz)      gunzip "$1"   ;;
        *.tar)     tar xf "$1"   ;;
        *.zip)     unzip "$1"    ;;
        *.7z)      7z x "$1"     ;;
        *) echo "'$1' cannot be extracted" ;;
    esac
}

# Quick backup of a file
backup() { cp "$1" "$1.backup.$(date +%Y%m%d_%H%M%S)"; }

# Find process by name
psfind() { ps aux | grep -i "$1" | grep -v grep; }

# Quick HTTP server
serve() { python3 -m http.server "${1:-8000}"; }
```

---

## 3. Custom Prompt (PS1)

```bash
# Colorful prompt with git branch
parse_git_branch() {
    git branch 2>/dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
}
export PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[33m\]$(parse_git_branch)\[\033[00m\]\$ '
```

---

## 4. SSH Config for Quick Connections

Create `~/.ssh/config`:

```
Host myserver
    HostName 192.168.1.100
    User murali-krishna
    Port 22
    IdentityFile ~/.ssh/mykey

Host *
    ServerAliveInterval 60
    AddKeysToAgent yes
```

Now just type `ssh myserver` instead of the full command.

### SSH Tricks

```bash
ssh-copy-id user@server                    # Copy key to server
ssh -L 8080:localhost:80 user@server       # Local port forwarding
ssh user@server 'uptime && df -h'         # Run remote command
scp -r ./folder user@server:/path/        # Copy files
```

---

## 5. Quick System Shortcuts

```bash
curl ifconfig.me                           # Public IP
openssl rand -base64 16                    # Random password
python3 -m http.server 8080               # File sharing server
curl cheat.sh/tar                          # Command cheat sheet
cal                                        # Calendar
du -sh */ | sort -rh                       # Directory sizes
```

---

## 6. `.inputrc` — Better Terminal Input

Create `~/.inputrc`:

```
set completion-ignore-case on
set show-all-if-ambiguous on
set colored-stats on
set bell-style none
"\e[A": history-search-backward
"\e[B": history-search-forward
```

---

## 7. Environment Variables

```bash
# Add to ~/.bashrc
export EDITOR="nano"
export HISTSIZE=10000
export HISTCONTROL=ignoredups:erasedups
export PATH="$HOME/.local/bin:$PATH"
```

---

## 💡 Tips

- Invest time in `~/.bashrc` — it pays off every day
- Use `.inputrc` arrow key history search — type `ssh` then `↑` to cycle SSH commands  
- `curl cheat.sh/command` is the fastest command lookup
- Back up dotfiles in a git repo: `~/.bashrc`, `~/.ssh/config`, `~/.inputrc`

---

[← Back to Index](../README.md) | [Next: Common Real-World Scenarios →](07-real-world-scenarios.md)
