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
```
--- 

### `Must Know`

````markdown
# Git & GitHub Cheat Sheet (Advanced)

## 🔑 Setup & Identity
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
````

## 📂 Repo & Remote

```bash
git init                     # Initialize repository
git clone <url>              # Clone remote repo locally
git remote -v                # Show remotes
git remote add origin <url>  # Add remote
git fetch origin             # Fetch updates (won't merge)
git fetch --all              # Fetch all remotes
```

## ✅ Checking Status & Differences

```bash
git status
git diff                    # Unstaged changes
git diff --staged           # Changes staged for commit
git log                     # Commit history
git log --oneline --graph --decorate --all
```

## ✍️ Staging & Committing

```bash
git add <file>
git add .                   # Stage all changes
git commit -m "message"
git commit --amend -m "new message"  # Edit last commit (local only)
```

## 🔀 Branching & Switching

```bash
git branch                 # List branches
git branch <name>          # Create branch
git checkout <branch>      # Switch branch
git switch <branch>        # Modern switch
git checkout -b <branch>   # Create & switch
git switch -c <branch>     # Create & switch (modern)
```

## 🔁 Merging & Rebasing

```bash
# Merge
git checkout main
git merge feature-branch
# Abort merge conflicts
git merge --abort

# Rebase
git checkout feature
git rebase main
# Interactive rebase (edit, squash, reorder)
git rebase -i HEAD~3
# Abort rebase
git rebase --abort
```

## 🧰 Stash

```bash
git stash push -m "WIP: message"   # Save changes
git stash list                       # List stashes
git stash apply stash@{0}            # Apply but keep stash
git stash pop                        # Apply and drop
git stash branch my-branch stash@{0} # Create branch from stash
```

## 🔙 Reset / Revert / Restore

```bash
# Reset moves HEAD (rewrites history locally)
git reset --soft HEAD~1   # Keep staged & working tree
git reset --mixed HEAD~1  # Keep working tree, unstage files (default)
git reset --hard HEAD~1   # Reset index + working tree (dangerous)

# Revert creates a new commit that undoes another commit
git revert <commit-sha>

# Newer commands for partial restore
git restore <file>                # discard working-tree changes to file
git restore --staged <file>       # unstage file
```

## ✨ Cherry-pick & Tag

```bash
# Pick a single commit to another branch
git cherry-pick <commit-sha>

# Tags
git tag v1.0                      # Lightweight
git tag -a v1.0 -m "Release 1.0" # Annotated tag
git push origin --tags             # Push tags
```

## 📦 Submodules & LFS

```bash
# Submodules (link another repo inside yours)
git submodule add <url> path
git submodule update --init --recursive

# Git LFS (large file support) -> install separately
# Example usage (after installing LFS):
git lfs track "*.psd"
git add .gitattributes
```

## 🧭 Recover & Inspect

```bash
git reflog                     # See movements of HEAD
# Recover lost branch/commit using commit SHA from reflog
# Example: git checkout -b recovered <sha>

git bisect start               # Binary search for a bad commit
git bisect bad <bad-sha-or-HEAD>
git bisect good <good-sha>
# After tests: git bisect reset
```

## ⚠️ Force & Safety

```bash
# Force push (dangerous when others use the branch)
git push --force-with-lease origin my-branch
# Safer than plain --force because it checks remote hasn't changed
```

## 🧩 Hooks & Automation

```text
# Git hooks are scripts in .git/hooks e.g., pre-commit, pre-push, post-merge
# Use tools like 'husky' (JS projects) to manage hooks across teams
```

## 🧪 Github-specific actions (short)

```text
# Pull requests, protected branches, reviews, actions (CI/CD), GitHub Pages, Releases
# Use branch protections to require reviews & pass checks before merging
```

## ⏱ Quick Troubleshooting

```bash
# Abort operations in progress
git merge --abort
git rebase --abort

# Undo a local file change
git checkout -- file.txt   # or: git restore file.txt

# Find commit by message
git log --all --grep "fix bug"
```

---

**Tip:** For any action that rewrites history (reset, rebase, commit --amend), **avoid running it on branches shared with others** unless your team agrees and you coordinate (or you understand forcing pushes).

```
```

---

