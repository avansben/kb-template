# System Auditing and Logs

## Purpose
Monitoring user activity and system events for security anomalies.

## Prereqs
Sudo access.

## Steps
1. Check recently logged-in users.
2. Audit failed login attempts.
3. Inspect system logs for errors.

## Commands
```bash
# List last logged in users
last

# Check failed login attempts
sudo faillog -a

# View system logs in real-time
sudo journalctl -f

# Search logs for specific keywords (e.g., "auth" or "error")
sudo journalctl -u ssh | grep "Failed password"

# View authentication logs
sudo tail -f /var/log/auth.log
```

## Verification
```bash
# Check for unusual IP addresses in auth.log
sudo grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c
```

## Rollback
N/A - Auditing is a read-only process.

## Notes
Consider installing `fail2ban` to automatically block IPs that show multiple failed login attempts.
