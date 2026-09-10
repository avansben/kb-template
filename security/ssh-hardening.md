# SSH Hardening

## Purpose
Securing the SSH daemon to prevent unauthorized access and brute-force attacks.

## Prereqs
Sudo access and a configured SSH key on the client machine.

## Steps
1. Disable password authentication.
2. Disable root login.
3. Change default SSH port.

## Commands
```bash
# Backup sshd_config
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak

# Edit config file
sudo nano /etc/ssh/sshd_config

# Ensure these lines are set:
# Port 2222
# PermitRootLogin no
# PasswordAuthentication no
# PubkeyAuthentication yes

# Restart SSH service
sudo systemctl restart ssh
```

## Verification
```bash
# Attempt to login with password (should fail)
ssh user@<IP> -o PubkeyAuthentication=no

# Attempt to login as root (should fail)
ssh root@<IP>
```

## Rollback
```bash
# Restore backup config
sudo cp /etc/ssh/sshd_config.bak /etc/ssh/sshd_config
sudo systemctl restart ssh
```

## Notes
ALWAYS test your SSH connection in a separate terminal window before closing your current session, or you might lock yourself out.
