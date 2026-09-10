# User and Group Management

## Purpose
Creating and managing users and groups for system access control.

## Prereqs
Sudo access.

## Steps
1. Create a new user.
2. Add user to a group (e.g., sudo).
3. Modify password expiration.

## Commands
```bash
# Create a new user with home directory
sudo useradd -m username

# Set password for the user
sudo passwd username

# Add user to a group
sudo usermod -aG sudo username

# Change user shell
sudo chsh -s /bin/zsh username

# Force user to change password on next login
sudo chage -d 0 username
```

## Verification
```bash
# Verify user exists and check groups
id username

# Check password expiry status
sudo chage -l username
```

## Rollback
```bash
# Remove user and home directory
sudo userdel -r username
```

## Notes
Always use `-aG` with `usermod` to append groups; otherwise, you will remove the user from all other groups.
