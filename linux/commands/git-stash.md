# Git Stashing

## Purpose
Temporarily shelving changes to work on something else without committing.

## Prereqs
Uncommitted changes in the working directory.

## Steps
1. Save current changes to the stash.
2. List all saved stashes.
3. Re-apply changes from the stash.

## Commands
```bash
# Stash all changes
git stash

# Stash with a message
git stash save "Work in progress for feature X"

# List all stashes
git stash list

# Apply the most recent stash and remove it from list
git stash pop

# Apply a specific stash by index
git stash apply stash@{1}

# Clear all stashes
git stash clear
```

## Verification
```bash
# Check status to see if changes returned
git status
```

## Rollback
```bash
# If pop caused conflicts, you can stash again
git stash
```

## Notes
`git stash pop` removes the entry from the list, while `git stash apply` keeps it.
