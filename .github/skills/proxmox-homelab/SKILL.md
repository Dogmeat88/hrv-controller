---
name: proxmox-homelab
description: 'Administer the Proxmox homelab host over SSH at root@192.168.1.104. Use for Proxmox VE host tasks, creating or troubleshooting LXC containers and VMs, storage, networking, local LLM workloads, and service troubleshooting. Prefer community helper scripts for guest creation when a maintained script exists.'
argument-hint: 'Describe the host, container, VM, storage, or networking task and whether the goal is diagnosis, maintenance, or a change.'
---

# Proxmox Homelab Administration

## When to Use
- Proxmox VE host administration or troubleshooting
- LXC container and VM lifecycle tasks
- Storage, backup, snapshot, or resource usage checks
- Network bridge, interface, or service troubleshooting on the virtualization host
- Investigation of local LLM containers or related homelab services

## Target Host
- SSH endpoint: `root@192.168.1.104`
- Platform: Proxmox VE
- Workload notes: host runs multiple containers for local LLMs and other services, plus at least one VM

## Version Context
- Verified 2026-06-28: Proxmox VE 9.2.0, `pve-manager` 9.2.2, kernel `7.0.2-6-pve`
- Keep host guidance tied to the live Proxmox release. Re-check CLI behavior, task output, and guest-management commands if Proxmox is upgraded.

## Operating Rules
- Inspect workload state before making changes to containers, VMs, storage, or networking.
- Prefer Proxmox-native tools over manual file edits when possible.
- For new LXC or VM creation, prefer the Proxmox community helper scripts when a maintained script exists for the requested guest. Review the script source and defaults first, then use native `pct` or `qm` commands for cases the community scripts do not cover.
- Call out risk before stopping guests, changing bridges, restarting cluster services, or editing storage/network config.
- Back up or snapshot before making impactful guest or storage changes when practical.

## Preferred Commands
- Connect: `ssh root@192.168.1.104`
- Host info: `pveversion -v`, `hostnamectl`, `uptime`
- Cluster and services: `systemctl status pvedaemon pveproxy pvestatd`, `journalctl -u pvedaemon -u pveproxy -u pvestatd --since -1h`
- Community guest creation: review available helper scripts at `https://community-scripts.github.io/ProxmoxVE/`, then run the matching community script when creating a supported LXC or VM
- Containers: `pct list`, `pct status <ctid>`, `pct config <ctid>`, `pct enter <ctid>`
- VMs: `qm list`, `qm status <vmid>`, `qm config <vmid>`
- Storage: `pvesm status`, `df -h`, `lsblk`
- Network: `ip addr`, `ip route`, `bridge link`, `bridge vlan show`
- Tasks and logs: `pvesh get /nodes/$(hostname)/tasks --limit 10`, `journalctl -xe`

## Safe Change Procedure
1. Connect to the host and identify the affected layer: host, storage, network, LXC, or VM.
2. Capture the current state with the narrowest relevant Proxmox commands.
3. For new guest creation, check whether a maintained community helper script already exists for the requested LXC or VM and prefer it when suitable.
4. If the change affects an existing guest, verify its ID, config, uptime, and recent logs first.
5. Apply the smallest viable change using the community helper script for supported guest creation, otherwise `pct`, `qm`, `pvesm`, `pvesh`, or targeted config edits only when necessary.
6. Restart only the impacted service or guest, not unrelated host services.
7. Validate runtime state, logs, and persistence across service reload or guest restart when relevant.

## High-Risk Actions
- Restarting networking on the host can interrupt management access and guest connectivity.
- Storage changes can affect multiple guests at once.
- Stopping or migrating containers running local LLMs may interrupt dependent services.
- Cluster-related service restarts can have broader effects than a single-host Linux service change.

## Validation Patterns
- Container health: `pct status <ctid>`, `pct exec <ctid> -- systemctl --no-pager --failed`, `pct exec <ctid> -- journalctl -p err -b --no-pager`
- VM health: `qm status <vmid>`, guest-specific service checks, and recent host logs
- Storage: `pvesm status`, `df -h`, task history for backup or snapshot operations
- Host load: `top`, `free -h`, `journalctl -p err -b --no-pager`

## Notes
- For local LLM containers, check CPU, RAM, disk, GPU passthrough, and model-serving logs before changing resource allocations.
- Prefer targeted guest restarts over host-level restarts unless the issue is clearly on the Proxmox host itself.
- Community helper scripts are preferred for initial provisioning only when they match the requested workload and remain actively maintained; use native Proxmox commands for follow-up tuning, unsupported guests, or highly customized builds.