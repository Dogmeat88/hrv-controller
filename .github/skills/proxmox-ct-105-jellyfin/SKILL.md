---
name: proxmox-ct-105-jellyfin
description: 'Administer Proxmox CT 105 jellyfin on root@192.168.1.104. Use for Jellyfin service health, media mount checks, logs, and LXC troubleshooting.'
argument-hint: 'Describe the Jellyfin task, such as service failure, media path issue, playback problem, or resource check.'
---

# Proxmox CT 105 Jellyfin

## Target
- Host: `root@192.168.1.104`
- Guest type: LXC container `105`
- Hostname: `jellyfin`
- OS: Ubuntu 24.04.4 LTS
- Config: 2 cores, 2 GB RAM, 16 GB rootfs, static IP `192.168.1.224`
- Mounts: `/mnt/usb-storage` mapped to `/mnt/media`

## Version Context
- Verified 2026-06-28: Ubuntu 24.04.4 LTS, Jellyfin `10.11.10+ubu2404`
- Keep playback, transcoding, and config guidance aligned to the live Jellyfin version. Re-check behavior after package upgrades.

## Use When
- Troubleshooting Jellyfin startup, playback, or library scan issues
- Validating media mounts and service logs inside the Jellyfin container

## Preferred Commands
- Status: `ssh root@192.168.1.104 'pct status 105 && pct exec 105 -- systemctl status jellyfin --no-pager'`
- Logs: `ssh root@192.168.1.104 'pct exec 105 -- journalctl -u jellyfin -n 200 --no-pager'`
- Media mount: `ssh root@192.168.1.104 'pct exec 105 -- df -h /mnt/media && pct exec 105 -- ls /mnt/media | head'`
- Shell: `ssh root@192.168.1.104 'pct enter 105'`

## Validation
- Verify CT `105` is running, `jellyfin` is active, and `/mnt/media` is present.
- Check service logs after changes.
- Confirm the affected library path or playback scenario behaves correctly.