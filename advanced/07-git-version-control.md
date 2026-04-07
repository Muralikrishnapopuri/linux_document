# 🌿 Git Version Control

Track changes, collaborate, and manage code history.

---

## 1. Setup & Configuration

```bash
# Install
sudo apt install git

# Configure identity
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

# Set default branch name
git config --global init.defaultBranch main

# Set default editor
git config --global core.editor "nano"       # Or vim, code, etc.

# View config
git config --list
```

---

## 2. Creating Repositories

```bash
# Initialize new repository
git init
git init project-name               # Create new directory with repo

# Clone existing repository
git clone https://github.com/user/repo.git
git clone https://github.com/user/repo.git custom-name
git clone --depth 1 https://github.com/user/repo.git  # Shallow clone
```

---

## 3. Basic Workflow

```bash
# Check status
git status
git status -s                       # Short format

# Stage files
git add file.txt                    # Stage specific file
git add .                           # Stage all changes
git add *.js                        # Stage by pattern
git add -p                          # Interactive staging

# Commit
git commit -m "Your message"
git commit -am "Stage + commit"     # Stage tracked files + commit
git commit --amend                  # Modify last commit

# View history
git log                             # Full log
git log --oneline                   # Compact log
git log --oneline --graph           # Visual branch graph
git log -n 5                        # Last 5 commits
git log --author="Alice"            # Filter by author
git log --since="2024-01-01"        # Filter by date
git log -- file.txt                 # History of a file
```

---

## 4. Branching

```bash
# View branches
git branch                          # Local branches
git branch -a                       # All (including remote)
git branch -v                       # With last commit info

# Create branch
git branch feature-login
git checkout -b feature-login       # Create and switch
git switch -c feature-login         # Modern syntax

# Switch branches
git checkout main
git switch main                     # Modern syntax

# Rename branch
git branch -m old-name new-name

# Delete branch
git branch -d feature-login         # Safe delete
git branch -D feature-login         # Force delete
```

---

## 5. Merging

```bash
# Merge branch into current branch
git checkout main
git merge feature-login

# Merge with commit message
git merge feature-login -m "Merge feature login"

# Abort a merge (if conflicts)
git merge --abort
```

### Resolving Conflicts

```bash
# After a conflict:
# 1. Open conflicted files and resolve manually
# 2. Stage resolved files
git add resolved-file.txt
# 3. Continue merge
git commit
```

---

## 6. Remote Repositories

```bash
# View remotes
git remote -v

# Add remote
git remote add origin https://github.com/user/repo.git

# Push
git push origin main                # Push to main
git push -u origin main             # Set upstream and push
git push                            # Push to upstream
git push --force                    # Force push (⚠️ dangerous)

# Pull (fetch + merge)
git pull origin main
git pull                            # From upstream

# Fetch (download without merging)
git fetch origin
git fetch --all

# Push a new branch
git push -u origin feature-branch

# Delete remote branch
git push origin --delete feature-branch
```

---

## 7. Undoing Changes

```bash
# Unstage a file
git restore --staged file.txt
git reset HEAD file.txt              # Older syntax

# Discard changes in working directory
git restore file.txt
git checkout -- file.txt             # Older syntax

# Reset to a previous commit
git reset --soft HEAD~1              # Keep changes staged
git reset --mixed HEAD~1             # Keep changes unstaged (default)
git reset --hard HEAD~1              # Discard all changes (⚠️)

# Revert a commit (creates new commit)
git revert abc1234

# Recover deleted commits
git reflog
git reset --hard abc1234
```

---

## 8. Stashing

Save work temporarily without committing.

```bash
git stash                            # Stash current changes
git stash save "work in progress"    # With message
git stash list                       # List all stashes
git stash pop                        # Apply and remove latest stash
git stash apply                      # Apply without removing
git stash apply stash@{2}           # Apply specific stash
git stash drop stash@{0}            # Delete specific stash
git stash clear                      # Delete all stashes
```

---

## 9. Viewing Changes

```bash
git diff                             # Unstaged changes
git diff --staged                    # Staged changes
git diff main..feature               # Between branches
git diff abc123..def456              # Between commits
git diff --stat                      # Summary of changes

git show abc1234                     # Show a specific commit
git blame file.txt                   # Who changed each line
```

---

## 10. Tags

```bash
git tag v1.0.0                       # Lightweight tag
git tag -a v1.0.0 -m "Release 1.0"  # Annotated tag
git tag                              # List tags
git push origin v1.0.0               # Push tag
git push origin --tags               # Push all tags
git tag -d v1.0.0                    # Delete local tag
git push origin --delete v1.0.0      # Delete remote tag
```

---

## 11. `.gitignore`

```bash
# Create .gitignore
cat > .gitignore << EOF
node_modules/
.env
*.log
dist/
.DS_Store
__pycache__/
*.pyc
.vscode/
EOF
```

---

## 12. Useful Aliases

```bash
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.lg "log --oneline --graph --all"
```

---

## 💡 Tips

- Commit early and often with clear messages
- Never force-push to shared branches
- Use `.gitignore` from the start of a project
- Use branches for features and bug fixes
- `git stash` before switching branches with uncommitted changes

---

[← Previous: Docker](06-docker-basics.md) | [Back to Index](../README.md) | [Next: Advanced Networking →](08-advanced-networking.md)
