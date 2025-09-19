# 09. Advanced — Deep Dive & Practical Examples
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


**Tip:** Use `git status` to see which files are conflicted and `git diff` to inspect differ

- For any action that rewrites history (reset, rebase, commit --amend), avoid running it on branches shared with others unless your team agrees and you coordinate (or you understand forcing pushes).
