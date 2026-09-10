# Git Branching and Merging

## Purpose
Managing feature development and integrating changes via branches.

## Prereqs
A git repository.

## Steps
1. Create and switch to a new branch.
2. Merge changes into the main branch.
3. Delete a merged branch.

## Commands
```bash
# Create and switch to new branch
git checkout -b feature/my-feature
# or
git switch -c feature/my-feature

# Switch back to main
git switch main

# Merge feature branch into main
git merge feature/my-feature

# Merge with squash (single commit)
git merge --squash feature/my-feature
```

## Verification
```bash
# List all local branches
git branch

# Check which branch is active
git branch --show-current
```

## Rollback
```bash
# Undo a merge (if not pushed)
git reset --hard ORIG_HEAD
```

## Notes
Use `git merge --no-ff` if you want to preserve the branch history in the graph.
