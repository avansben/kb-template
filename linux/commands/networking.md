# Networking Commands

## Purpose
Managing and troubleshooting network connectivity, interfaces, and DNS.

## Prereqs
Sudo access for some commands (ip, ifconfig).

## Steps
1. Check interface status.
2. Verify IP address.
3. Test connectivity to a remote host.
4. Check DNS resolution.

## Commands
```bash
# Check IP address and interfaces
ip addr show
# or
ifconfig

# Check routing table
ip route show
# or
route -n

# Test connectivity
ping -c 4 8.8.8.8

# Check DNS resolution
dig google.com
# or
nslookup google.com

# List open ports and listening sockets
ss -tulpn
# or
netstat -tulpn
```

## Verification
```bash
# Verify interface is UP
ip link show

# Check if a specific port is open on a remote server
nc -zv 8.8.8.8 53
```

## Rollback
N/A - These are read-only or transient commands.

## Notes
`ip` command is the modern replacement for `ifconfig` and `route`.
