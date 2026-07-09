---
name: proxmox-ct-111-frigate
description: 'Administer Proxmox CT 111 frigate on root@192.168.1.104. Use for the Frigate Docker workload, storage mounts, logs, and LXC troubleshooting.'
argument-hint: 'Describe the Frigate task, such as camera ingestion, Docker health, storage, logs, or performance.'
---

# Proxmox CT 111 Frigate

## Target
- Host: `root@192.168.1.104`
- Guest type: LXC container `111`
- Hostname: `frigate`
- OS: Debian GNU/Linux 13
- Config: 4 cores, 4 GB RAM, 16 GB rootfs, static IP `192.168.1.152`
- Mounts: `/mnt/pve/CCTV/frigate` mapped to `/media/frigate`
- Observed runtime: Docker container `frigate`

## Version Context
- Verified 2026-06-28: Debian GNU/Linux 13
- Observed image: `ghcr.io/blakeblackshear/frigate:stable`
- Because the container uses a floating `stable` tag, re-check the live image version or digest before giving version-specific Frigate guidance.

## Use When
- Troubleshooting Frigate camera ingestion, storage, or container health
- Inspecting Docker logs and media storage paths inside CT `111`

## Preferred Commands
- Status: `ssh root@192.168.1.104 'pct status 111 && pct exec 111 -- docker ps'`
- Logs: `ssh root@192.168.1.104 'pct exec 111 -- docker logs --tail 200 frigate'`
- Storage: `ssh root@192.168.1.104 'pct exec 111 -- df -h /media/frigate && pct exec 111 -- ls /media/frigate | head'`
- Shell: `ssh root@192.168.1.104 'pct enter 111'`

## Validation
- Confirm CT `111` is running and the `frigate` container is present in `docker ps`.
- Verify `/media/frigate` is mounted and writable as expected.
- Re-test the affected camera or recording path after changes.