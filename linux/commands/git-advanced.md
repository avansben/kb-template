# Advanced Git Operations

## Purpose
Handling complex history modifications, cherry-picking, and recovery.

## Prereqs
A git repository with commits to manipulate.

## Steps
1. Clean up history using interactive rebase.
2. Move a specific commit from one branch to another.
3. Recover a "lost" commit.

## Commands
```bash
# Interactive rebase of last 3 commits
git rebase -i HEAD~3

# Cherry-pick a specific commit
git cherry-pick <commit-hash>

# View all actions in the local repo (recovery)
git reflog

# Reset branch to a specific state from reflog
git reset --hard <reflog-hash>
```

## Verification
```bash
# Check commit history
git log --oneline --graph --all
```

## Rollback
```bash
# Abort an ongoing rebase
git rebase --abort

# Undo a cherry-pick
git reset --hard HEAD~1
```

## Notes
Interactive rebase rewrites history. Never rebase commits that have been pushed to a shared branch.
