### `09-advanced.md`

````markdown
# Advanced — Deep Dive & Practical Examples

This file contains deeper explanations of Git internals, advanced commands, recommended workflows, conflict resolution strategies, and practice exercises with solutions.

---

## 1) Git internals (simple explanation)

- **Working Directory**: The files on your filesystem you edit.
- **Staging Area (Index)**: Where files go when you `git add` — a snapshot that will become the next commit.
- **Repository (.git)**: Where commits, refs, objects, hooks, and config live.
- **HEAD**: Pointer to the current branch tip (the commit you currently have checked out).
- **Commit SHA**: A unique identifier for each commit (hash). Think of it as a fingerprint for a checkpoint.

**Analogy**: Working directory = your desk, staging area = stack of papers ready to file, `.git` = locked cabinet with all historical folders.

---

## 2) Reset vs Revert vs Restore — When to use

- **git reset**: Moves `HEAD` and optionally changes index and working tree. *Rewrites history*. Use for local cleanup before sharing. Examples:
  - `git reset --soft HEAD~1` → move HEAD back 1 commit, keep changes staged.
  - `git reset --mixed HEAD~1` → default; unstage changes but keep them in working dir.
  - `git reset --hard HEAD~1` → LOST local changes (index + working tree match new HEAD).

- **git revert <sha>**: Creates a *new* commit that undoes the given commit. Safe on shared branches.

- **git restore** (new): Can restore files from index/HEAD to working tree safely (non-destructive to history).

**Rule of thumb**: If commit is already pushed/shared → prefer `git revert`. If commit is local and not pushed → `git reset` or `git commit --amend` or interactive rebase.

---

## 3) Rebase (regular & interactive)

- `git rebase main` (on feature branch) reapplies your commits on top of `main`, producing a linear history.
- Interactive rebase: `git rebase -i HEAD~N` lets you reword, squash, reorder, or drop commits.

**Safety**: Rewriting history (rebase) that others rely on causes problems — rebase only local/private branches or coordinate with the team.

**Steps for interactive rebase**:
1. `git checkout feature`
2. `git rebase -i HEAD~3`  # edit the list
3. Change `pick` to `squash`/`fixup`/`reword` as needed
4. Resolve any conflicts, `git rebase --continue`
5. When done, `git push --force-with-lease` (only if safe)

---

## 4) Merge vs Rebase — Which to choose?

- **Merge**: Keeps history as-is and adds a merge commit; good for preserving true chronological history and for long-lived branches.
- **Rebase**: Creates a straight, linear history; good for small, feature branches that you want to keep tidy.

**Team guidelines**: Choose one convention (e.g., GitHub Flow: rebase/squash locally + merge via Pull Request; or Git Flow for release cycles). Document it in CONTRIBUTING.md.

---

## 5) Stash — practical uses

- `git stash push -m "WIP: quick change"` save uncommitted changes
- `git stash list` to view stashes
- `git stash apply` or `git stash pop` to restore
- `git stash branch <branch> stash@{0}` to create a new branch from stash

**Analogy**: Put your work on a sticky pad to free your desk for something urgent, then stick it back later.

---

## 6) Cherry-pick

- `git cherry-pick <commit-sha>` applies the changes from a commit to your current branch without merging the whole branch.

**Use-case**: You fixed a bug on `main` and want the same fix in `release/1.0` without merging other feature changes.

---

## 7) Reflog — recover lost commits

- `git reflog` shows where HEAD pointed over time (local operations included).
- If you accidentally `git reset --hard`, find the old SHA in reflog and run:
  - `git checkout -b recovered <sha>`

**Analogy**: Reflog is a CCTV of your Git HEAD movements.

---

## 8) Bisect — find the commit that introduced a bug

1. `git bisect start`
2. `git bisect bad` (current broken commit)
3. `git bisect good <good-commit-sha>`
4. Git checks out a middle commit — run your test (manual or automated)
5. Mark `git bisect good` or `git bisect bad` until it finds offending commit
6. `git bisect reset` to return to original branch

**Super helpful** when you have lots of commits and a failing test.

---

## 9) Tags & Releases

- `git tag -a v1.0 -m "Release v1.0"`
- `git push origin --tags`
- Use annotated tags for releases (include message & creator metadata)
- In GitHub, add a Release to attach binaries and release notes

---

## 10) Submodules (short)

- Add: `git submodule add <url> path`
- Init & update: `git submodule update --init --recursive`

**Complexity**: Submodules link to specific commits of the external repo — handy but requires care in team workflows.

---

## 11) Hooks

- Hooks live in `.git/hooks` (client-side) — pre-commit, pre-push, post-merge.
- Use hooks to run linters, tests, or enforce commit message style.
- For team-wide hooks, use tools like `husky` (JavaScript) or CI checks in GitHub Actions.

---

## 12) Conflicts — how to resolve step-by-step

1. During `git merge` or `git rebase`, conflicts are reported.
2. Open files with conflict markers `<<<<<<`, `======`, `>>>>>>`.
3. Edit to keep desired changes.
4. `git add <file>` after resolving each file.
5. Continue the operation:
   - If merging: `git commit` (if needed; some merges auto-commit)
   - If rebasing: `git rebase --continue`
6. If stuck, you can abort:
   - `git merge --abort` or `git rebase --abort`

**Tip:** Use `git status` to see which files are conflicted and `git diff` to inspect differences.

---

## 13) Working with remotes safely

- Pull vs Fetch:
  - `git fetch` downloads objects and refs but does not alter working directory.
  - `git pull` = `git fetch` + merge (or `--rebase`), which can change working tree.

- Set upstream and push once:
  - `git push --set-upstream origin feature-branch`

- Force push safely:
  - `git push --force-with-lease` checks remote didn't change unexpectedly.

---

## 14) Branch protection & code review best practices (GitHub)

- Protect `main` or `release/*` branches: require PR reviews, passing CI, signed commits if needed.
- Use PR templates and `.github/` files (CODEOWNERS) to guide reviewers.

---
````
## 15) Example workflows (copyable commands)

### A. Simple feature workflow (GitHub Flow)
```bash
# Local
git checkout -b feature/login
# work, add, commit
git add .
git commit -m "feat: add login form"
git push --set-upstream origin feature/login
# Open PR on GitHub
# After review, merge via GitHub (Squash & Merge or Merge Commit)
```

### B. Cleaning up commits before pushing (interactive rebase)

```bash
# Suppose you made 5 small commits and want 1 clean commit
git rebase -i HEAD~5
# Mark top as "pick" and others as "squash" then save
# Resolve conflicts if any
git push --force-with-lease origin feature/login
```

### C. Recover an accidentally deleted branch

```bash
# Find SHA in reflog
git reflog
# Create branch at lost SHA
git checkout -b recover-branch <sha>
git push origin recover-branch
```

### D. Cherry-pick a fix to another branch

```bash
# On target branch
git checkout release/1.0
git cherry-pick <fix-commit-sha>
git push origin release/1.0
```

---

## 16) Advanced Exercises (with solutions)

### Exercise A — Recover a lost commit (easy)

**Task**: You accidentally reset head and lost a commit. Recover it.
**Solution**:

```bash
# Use reflog
git reflog
# Identify SHA before reset
git checkout -b recovered <sha>
```

### Exercise B — Squash recent commits (intermediate)

**Task**: Combine the last 4 commits into 1 tidy commit.
**Solution**:

```bash
git rebase -i HEAD~4
# change pick -> squash for last 3 commits
# save and force-push (if needed)
git push --force-with-lease
```

### Exercise C — Use bisect to find a bad commit (intermediate)

**Task**: A test passes on version `A` but fails on `HEAD`. Use bisect to find first bad commit.
**Solution**:

```bash
git bisect start
git bisect bad HEAD
git bisect good <sha-of-known-good>
# follow prompts, test each commit, mark good/bad
git bisect reset
```

### Exercise D — Safely update a shared branch after rewriting history (advanced)

**Task**: You rebased your feature branch to clean up commits but your branch is on remote and colleagues may have pulled it.
**Solution**:

```text
# Preferred: Communicate to team that you'll rewrite history and coordinate
# Then push with lease
git push --force-with-lease origin feature/cleaned
```

**Alternative**: Instead of rewriting history, create a fresh branch and merge it normally so others keep working.

---

## 17) Further reading & learning path

1. Git official docs — deep reference of plumbing & porcelain commands
2. Pro Git (book) — great for mastering concepts and workflows
3. Hands-on practice: contribute to open-source projects (small issues first)

---

## 18) Quick reference — common mistakes to avoid

* Do **not** `git push --force` to shared branches without coordination.
* Avoid large binary files in Git (use Git LFS for big assets).
* Don't ignore `git status` — it tells you exactly what’s happening.
* When in doubt, make a backup branch: `git branch backup` before big operations.

---


```
```

---
