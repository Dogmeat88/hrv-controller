---
name: proxmox-ct-110-flow-collector
description: 'Administer Proxmox CT 110 flow-collector on root@192.168.1.104. Use for flow collection, nfdump service health, logs, and LXC troubleshooting.'
argument-hint: 'Describe the flow-collector issue, such as missing data, service startup, storage, or log inspection.'
---

# Proxmox CT 110 Flow Collector

## Target
- Host: `root@192.168.1.104`
- Guest type: LXC container `110`
- Hostname: `flow-collector`
- OS: Debian GNU/Linux 13
- Config: 1 core, 512 MB RAM, 4 GB rootfs, static IP `192.168.1.110`
- Observed runtime: `nfdump` active

## Version Context
- Verified 2026-06-28: Debian GNU/Linux 13, `nfdump` 1.7.5-release
- Keep collector and file-format guidance tied to the live `nfdump` version. Re-check flags and output format after upgrades.

## Use When
- Troubleshooting network-flow ingestion, storage, or retention
- Checking `nfdump` process health, logs, and data directories inside CT `110`

## Preferred Commands
- Status: `ssh root@192.168.1.104 'pct status 110 && pct exec 110 -- systemctl status nfdump --no-pager'`
- Logs: `ssh root@192.168.1.104 'pct exec 110 -- journalctl -u nfdump -n 200 --no-pager'`
- Shell: `ssh root@192.168.1.104 'pct enter 110'`
- Failed units: `ssh root@192.168.1.104 'pct exec 110 -- systemctl --failed --no-pager'`

## Validation
- Verify CT `110` is running and `nfdump` is active.
- Check for recent flow files and service logs after changes.
- Re-test the export path from the upstream router or collector source if data is missing.