# Process Management

## Purpose
Monitoring system resources and managing running processes.

## Prereqs
Sudo access for killing processes owned by other users.

## Steps
1. Identify resource-heavy processes.
2. Find the PID of a specific process.
3. Terminate a hanging process.

## Commands
```bash
# Interactive process viewer
top
# or
htop

# List all running processes
ps aux

# Search for a specific process by name
ps aux | grep nginx

# Send termination signal (Graceful)
kill -15 <PID>

# Force kill (Immediate)
kill -9 <PID>

# Change priority of a process
renice +10 -p <PID>
```

## Verification
```bash
# Check if process is still running
ps -p <PID>

# Monitor CPU/Memory usage
free -m
```

## Rollback
N/A - Process termination cannot be undone without restarting the service.

## Notes
Avoid `kill -9` unless necessary, as it doesn't allow the process to clean up.
