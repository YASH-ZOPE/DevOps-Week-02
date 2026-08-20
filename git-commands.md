# 🔀 Git Commands Cheatsheet & Reference Guide

A practical reference for Git version control commands, workflows, and best practices.

---

## 🛠️ 1. Setup & Configuration

Configure user details and system-wide Git settings:

```bash
# Set global username and email
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Set default branch name to main
git config --global init.defaultBranch main

# Enable colored output
git config --global color.ui auto

# List all current configuration settings
git config --list --show-origin
```

---

## 🚀 2. Initializing & Cloning Repositories

```bash
# Initialize a new Git repository in the current directory
git init

# Clone a remote repository locally
git clone https://github.com/user/repository.git

# Clone a specific branch
git clone -b feature-branch https://github.com/user/repository.git
```

---

## 📝 3. Staging & Committing Changes

```bash
# Check working tree status
git status

# Stage specific file(s)
git add filename.ext

# Stage all changed and new files
git add .

# Commit staged changes with a descriptive message
git commit -m "feat: add user authentication module"

# Amend the last commit (message or staged files)
git commit --amend -m "feat: updated user auth module with token validation"

# View diff of unstaged changes
git diff

# View diff of staged changes
git diff --staged
```

---

## 🌿 4. Branching & Merging

```bash
# List all local branches (* indicates active branch)
git branch

# List local and remote branches
git branch -a

# Create a new branch
git branch feature-login

# Switch to a branch
git switch feature-login
# or (legacy command):
git checkout feature-login

# Create and switch to a new branch in one step
git switch -c feature-login

# Merge a branch into the current active branch
git merge feature-login

# Delete a local branch (safe delete)
git branch -d feature-login

# Force delete a local branch
git branch -D feature-login
```

---

## 🌐 5. Working with Remotes

```bash
# View remote repositories
git remote -v

# Add a remote repository URL
git remote add origin https://github.com/user/repository.git

# Fetch latest changes from remote without merging
git fetch origin

# Pull latest changes from remote and merge into current branch
git pull origin main

# Push local branch to remote repository
git push -u origin main

# Push changes to remote branch
git push origin feature-login

# Delete a remote branch
git push origin --delete feature-login
```

---

## 📦 6. Stashing Temporary Changes

```bash
# Stash uncommitted changes (working directory & staged)
git stash

# Stash changes with a custom description
git stash save "WIP: login page styling"

# List all stashed changes
git stash list

# Apply the latest stash without removing it from stash list
git stash apply

# Apply and remove the latest stash
git stash pop

# Clear all stashes
git stash clear
```

---

## ⏪ 7. Undoing & History Management

```bash
# Unstage a file keeping modifications in working directory
git restore --staged filename.ext

# Discard changes in working directory for a file
git restore filename.ext

# Revert a commit by creating a new inverse commit
git revert <commit-hash>

# Soft Reset: Move HEAD to commit, keeping changes staged
git reset --soft <commit-hash>

# Mixed Reset (default): Move HEAD, keeping changes in working tree (unstaged)
git reset --mixed <commit-hash>

# Hard Reset: Move HEAD and discard all changes (CAUTION: destructible)
git reset --hard <commit-hash>
```

---

## 📜 8. Logs & Inspection

```bash
# View commit history
git log

# One-line commit history log graph
git log --oneline --graph --all

# View detailed commit details
git show <commit-hash>

# View who modified which line in a file
git blame filename.ext
```

---

## 💡 Git Best Practices

- **Atomic Commits:** Make small, frequent commits focused on a single logical change.
- **Meaningful Messages:** Follow conventional commit standards (e.g., `feat:`, `fix:`, `docs:`, `chore:`).
- **Branch Strategy:** Maintain a clean `main` branch; perform work in isolated feature branches.
- **Pull Before Push:** Always pull recent upstream changes before pushing to minimize merge conflicts.
