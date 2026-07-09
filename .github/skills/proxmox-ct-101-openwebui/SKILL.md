---
name: proxmox-ct-101-openwebui
description: 'Administer Proxmox CT 101 openwebui on root@192.168.1.104. Use for Open WebUI service health, logs, upgrades, and LXC troubleshooting.'
argument-hint: 'Describe the Open WebUI problem or task, such as startup failure, UI access, logs, or configuration.'
---

# Proxmox CT 101 Open WebUI

## Target
- Host: `root@192.168.1.104`
- Guest type: LXC container `101`
- Hostname: `openwebui`
- OS: Debian GNU/Linux 13
- Config: 2 cores, 2 GB RAM, 50 GB rootfs, DHCP on `vmbr0`

## Version Context
- Verified 2026-06-28: Debian GNU/Linux 13; Open WebUI is installed via `/root/.local/bin/open-webui`
- Exact Open WebUI version was not returned cleanly from the shell probe. Re-check the app version before giving version-specific UI, API, or config-path guidance.

## Use When
- Troubleshooting Open WebUI startup, UI access, or service logs
- Inspecting configuration or package state inside the Open WebUI container

## Preferred Commands
- Status: `ssh root@192.168.1.104 'pct status 101 && pct exec 101 -- systemctl status open-webui --no-pager'`
- Logs: `ssh root@192.168.1.104 'pct exec 101 -- journalctl -u open-webui -n 200 --no-pager'`
- Shell: `ssh root@192.168.1.104 'pct enter 101'`
- Failed units: `ssh root@192.168.1.104 'pct exec 101 -- systemctl --failed --no-pager'`

## Validation
- Verify CT `101` is running and `open-webui` is active.
- Check `journalctl` for bind, dependency, or upgrade errors.
- If UI access is the issue, validate the listening service and upstream model endpoint configuration.