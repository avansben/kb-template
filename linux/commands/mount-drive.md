cat > ~/kb/templates/note-template.md <<'EOF'
# Title
Detailed Drive Mounting Instructions

## Purpose
This documents details mounting a drive in it's various command formats

## Prereqs
Accounts, IPs, tools, access, assumptions.

## Steps
1. list block devices
2. create a mount point
3. mount the drive
4. unmount the drives
5. automount at boot
   1. get UUID
   2. edit /etc/fstab

## Commands
```bash
lsblk -lf
sudo mkdir -p /mnt/drive
sudo mount <device> <mountpoint>
sudo umount /mnt/mydrive
# Automount at boot
sudo blkid
sudo nano /etc/fstab
UUID=YOUR-UUID-HERE  /mnt/mydrive  <mountpoint>  defaults,nofail  0  0
```

## Verification
```bash
# checks / show commands / curl tests / ping tests
```

## Rollback
```bash
# undo or recovery commands
```

## Notes
Extra context, gotchas, links, ticket refs.
