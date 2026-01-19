# Git & GitHub – Practical Commands and Tips (Ubuntu • Terminal • VS Code)

## Assumptions
- OS: Ubuntu 22.04+  
- Git ≥ 2.40  
- VS Code installed with Git integration  
- GitHub account + SSH access  

## Dependencies (why they matter)
- `git`: version control engine  
- `openssh-client`: secure GitHub auth (avoids passwords)  
- VS Code Git UI: visual diff, staging, conflict resolution  

---

## 1. One-Time Setup

1. Install Git  
   `sudo apt update && sudo apt install -y git`

2. Configure identity  
   `git config --global user.name "Your Name"`  
   `git config --global user.email "you@email.com"`

3. Set default branch  
   `git config --global init.defaultBranch main`

4. Enable helpful defaults  
   `git config --global pull.rebase true`  
   `git config --global fetch.prune true`  
   `git config --global rerere.enabled true`

---

## 2. SSH for GitHub (recommended)

1. Generate key  
   `ssh-keygen -t ed25519 -C "you@email.com"`

2. Start agent  
   `eval "$(ssh-agent -s)"`  
   `ssh-add ~/.ssh/id_ed25519`

3. Copy key  
   `cat ~/.ssh/id_ed25519.pub`

4. Add to GitHub → Settings → SSH keys

5. Verify  
   `ssh -T git@github.com`

---

## 3. Create or Clone Repositories

- New repo (local)  
  `git init`

- Clone  
  `git clone git@github.com:user/repo.git`

- Add remote manually  
  `git remote add origin git@github.com:user/repo.git`

- Verify remotes  
  `git remote -v`

---

## 4. Daily Workflow (Minimal)

1. Check status  
   `git status`

2. Stage files  
   `git add file.txt`  
   `git add .`

3. Commit  
   `git commit -m "clear, imperative message"`

4. Pull latest  
   `git pull`

5. Push  
   `git push`

---

## 5. Branching (Safe Pattern)

- Create + switch  
  `git switch -c feature/auth`

- List branches  
  `git branch`

- Switch  
  `git switch main`

- Delete local  
  `git branch -d feature/auth`

- Delete remote  
  `git push origin --delete feature/auth`

---

## 6. Commit Best Practices

- One logical change per commit  
- Message format:  
  `type: short summary`  
  examples: `fix: handle null token`, `feat: add QR scan`

- Amend last commit  
  `git commit --amend`

---

## 7. Undoing Things (Critical)

- Unstage file  
  `git restore --staged file.txt`

- Discard local changes  
  `git restore file.txt`

- Undo last commit (keep changes)  
  `git reset --soft HEAD~1`

- Undo last commit (wipe changes)  
  `git reset --hard HEAD~1`

- Revert pushed commit (safe)  
  `git revert <commit_hash>`

---

## 8. Viewing History & Diffs

- Log (compact)  
  `git log --oneline --graph --all`

- File history  
  `git log -- file.txt`

- Diff unstaged  
  `git diff`

- Diff staged  
  `git diff --staged`

---

## 9. Stash (Context Switching)

- Save work  
  `git stash`

- List  
  `git stash list`

- Apply latest  
  `git stash pop`

- Apply specific  
  `git stash apply stash@{1}`

---

## 10. Rebasing (Clean History)

- Rebase feature onto main  
  `git switch feature/x`  
  `git rebase main`

- Interactive rebase (last 5)  
  `git rebase -i HEAD~5`

- Abort rebase  
  `git rebase --abort`

---

## 11. Merge Conflicts (Fast Resolution)

1. Open conflicted files in VS Code  
2. Use “Accept Current / Incoming / Both”  
3. Verify manually  
4. Stage resolved files  
   `git add .`  
5. Continue  
   `git rebase --continue` or `git commit`

---

## 12. Tags & Releases

- Create tag  
  `git tag v1.0.0`

- Push tags  
  `git push origin --tags`

---

## 13. .gitignore (Must-Have)

Common entries:
- `node_modules/`
- `.env`
- `.idea/`
- `.vscode/`
- `build/`
- `dist/`

Apply after tracking mistake:
`git rm -r --cached node_modules`

---

## 14. VS Code Tips

- `Ctrl+Shift+G`: Git panel  
- Stage/unstage via UI  
- Inline diff editor  
- Conflict resolution buttons  
- Timeline view for file history  

---

## 15. Common Failure Modes & Fixes

- Push rejected  
  Fix: `git pull --rebase`

- Detached HEAD  
  Fix: `git switch main`

- Wrong commit message  
  Fix: `git commit --amend`

- Accidentally committed secrets  
  Fix immediately: rotate secret, then `git filter-repo`

- Large files rejected  
  Fix: remove file + consider Git LFS

---

## 16. Verification Checklist

- `git status` clean  
- `git log --oneline -5` sane  
- `git remote -v` correct  
- `ssh -T git@github.com` works  

---

## 17. Golden Rules

1. Never work directly on `main`  
2. Pull before you push  
3. Commit small, commit often  
4. Revert > reset for shared history  
5. Read diffs before committing
