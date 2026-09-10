# UFW Firewall Setup

## Purpose
Implementing a basic firewall to control incoming and outgoing traffic.

## Prereqs
UFW installed (standard on Ubuntu).

## Steps
1. Set default policies.
2. Allow specific services (SSH, HTTP, HTTPS).
3. Enable the firewall.

## Commands
```bash
# Set defaults: deny all incoming, allow all outgoing
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Allow SSH (on custom port if changed)
sudo ufw allow 22/tcp
# or
sudo ufw allow 2222/tcp

# Allow HTTP/HTTPS
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Enable Firewall
sudo ufw enable

# Check status
sudo ufw status verbose
```

## Verification
```bash
# Test if HTTP is accessible from outside
curl -I http://<IP>
```

## Rollback
```bash
# Disable firewall
sudo ufw disable

# Reset all rules
sudo ufw reset
```

## Notes
Ensure SSH is allowed BEFORE enabling UFW to avoid losing remote access.
