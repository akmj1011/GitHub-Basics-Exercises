# 08. Git & GitHub Cheat Sheet

## 🔑 Basic Commands
```bash
git init                 # Start a new repo
git clone <url>          # Copy repo to system
git status               # Check changes
git add .                # Stage changes
git commit -m "message"  # Commit changes
git push                 # Upload to GitHub
git pull                 # Get updates from GitHub
git checkout -b new-branch # Create & switch to branch
```

## ⚙️ Setup & Identity
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

# Set editor
git config --global core.editor "code --wait"  # VS Code example

# Helpful aliases
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.st status
git config --global alias.ci commit
