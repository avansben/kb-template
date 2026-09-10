# File Permissions and Ownership

## Purpose
Managing access control for files and directories on Linux.

## Prereqs
Sudo access for changing ownership of files.

## Steps
1. View current permissions.
2. Change file permissions (read/write/execute).
3. Change file owner or group.

## Commands
```bash
# View permissions in long format
ls -l

# Change permissions (Symbolic)
chmod u+x script.sh # Add execute for owner
chmod g-w file.txt  # Remove write for group

# Change permissions (Numeric)
chmod 644 file.txt  # rw-r--r--
chmod 755 folder/   # rwxr-xr-x

# Change owner
sudo chown user:group file.txt

# Change owner recursively
sudo chown -R user:group folder/
```

## Verification
```bash
# Verify changes
ls -l file.txt
```

## Rollback
```bash
# Reset to common defaults
chmod 644 file.txt
chmod 755 folder/
```

## Notes
Be extremely careful with `chmod 777` as it allows anyone to read, write, and execute the file.
