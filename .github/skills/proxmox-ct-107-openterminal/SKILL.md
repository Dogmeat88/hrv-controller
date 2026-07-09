---
name: proxmox-ct-107-openterminal
description: 'Administer Proxmox CT 107 openterminal on root@192.168.1.104. Use for the open-terminal Docker workload, logs, access issues, and LXC troubleshooting.'
argument-hint: 'Describe the terminal service issue, such as Docker startup, UI access, logs, or configuration.'
---

# Proxmox CT 107 OpenTerminal

## Target
- Host: `root@192.168.1.104`
- Guest type: LXC container `107`
- Hostname: `openterminal`
- OS: Debian GNU/Linux 13
- Config: 2 cores, 2 GB RAM, 10 GB rootfs, DHCP on `vmbr0`
- Observed runtime: Docker container `open-terminal`

## Version Context
- Verified 2026-06-28: Debian GNU/Linux 13
- Observed image: `ghcr.io/open-webui/open-terminal:latest`
- Because the container uses a floating tag, re-check the live image version or digest before giving version-specific guidance.

## Use When
- Troubleshooting the browser-accessible terminal service
- Inspecting Docker container logs, restarts, or port exposure inside CT `107`

## Preferred Commands
- Status: `ssh root@192.168.1.104 'pct status 107 && pct exec 107 -- docker ps'`
- Logs: `ssh root@192.168.1.104 'pct exec 107 -- docker logs --tail 200 open-terminal'`
- Shell: `ssh root@192.168.1.104 'pct enter 107'`
- Restart: `ssh root@192.168.1.104 'pct exec 107 -- docker restart open-terminal'`

## Validation
- Confirm CT `107` is running and `open-terminal` is present in `docker ps`.
- Review container logs after any configuration change.
- Test the terminal web access path that was failing.