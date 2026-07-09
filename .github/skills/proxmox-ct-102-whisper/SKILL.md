---
name: proxmox-ct-102-whisper
description: 'Administer Proxmox CT 102 whisper on root@192.168.1.104. Use for whisper-ha and whisper-webui containers, Docker troubleshooting, logs, and transcription service checks.'
argument-hint: 'Describe the transcription or container issue, such as Docker startup, service access, logs, or resource usage.'
---

# Proxmox CT 102 Whisper

## Target
- Host: `root@192.168.1.104`
- Guest type: LXC container `102`
- Hostname: `whisper`
- OS: Debian GNU/Linux 13
- Config: 2 cores, 2 GB RAM, 24 GB rootfs, DHCP on `vmbr0`
- Observed runtime: Docker containers `whisper-ha` and `whisper-webui`

## Version Context
- Verified 2026-06-28: Debian GNU/Linux 13
- Observed images: `lscr.io/linuxserver/faster-whisper:latest` and `fedirz/faster-whisper-server:latest-cuda`
- Because these containers use floating tags, re-check the live image digest or tag before giving version-specific Docker or application guidance.

## Use When
- Troubleshooting the Whisper stack or its Docker containers
- Inspecting logs, images, ports, or restarts related to transcription workloads

## Preferred Commands
- Status: `ssh root@192.168.1.104 'pct status 102 && pct exec 102 -- docker ps'`
- Logs: `ssh root@192.168.1.104 'pct exec 102 -- docker logs --tail 200 whisper-ha'`
- Web UI logs: `ssh root@192.168.1.104 'pct exec 102 -- docker logs --tail 200 whisper-webui'`
- Shell: `ssh root@192.168.1.104 'pct enter 102'`

## Validation
- Confirm CT `102` is running and both Docker containers are up.
- Review container logs after any change.
- Test a narrow transcription request or the web UI path that was failing.