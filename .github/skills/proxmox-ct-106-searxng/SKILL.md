---
name: proxmox-ct-106-searxng
description: 'Administer Proxmox CT 106 searxng on root@192.168.1.104. Use for SearXNG service health, logs, config, and LXC troubleshooting.'
argument-hint: 'Describe the SearXNG task, such as search failure, service health, reverse proxy access, or configuration.'
---

# Proxmox CT 106 SearXNG

## Target
- Host: `root@192.168.1.104`
- Guest type: LXC container `106`
- Hostname: `searxng`
- OS: Debian GNU/Linux 13
- Config: 2 cores, 2 GB RAM, 7 GB rootfs, DHCP on `vmbr0`

## Version Context
- Verified 2026-06-28: Debian GNU/Linux 13, SearXNG `2026.5.31+300695de5`
- Use config, engine, and troubleshooting guidance that matches this SearXNG version. Re-check paths and settings semantics after upgrades.

## Use When
- Troubleshooting SearXNG startup, engine failures, or access issues
- Inspecting service logs and container config for the SearXNG workload

## Preferred Commands
- Status: `ssh root@192.168.1.104 'pct status 106 && pct exec 106 -- systemctl status searxng --no-pager'`
- Logs: `ssh root@192.168.1.104 'pct exec 106 -- journalctl -u searxng -n 200 --no-pager'`
- Shell: `ssh root@192.168.1.104 'pct enter 106'`
- Failed units: `ssh root@192.168.1.104 'pct exec 106 -- systemctl --failed --no-pager'`

## Validation
- Verify CT `106` is running and `searxng` is active.
- Review logs for upstream engine, bind, or dependency errors.
- Test the specific search path or endpoint that was failing.