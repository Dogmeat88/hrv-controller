# Project Guidelines

## Scope
This workspace is for generic local system administration across a small homelab.
Primary systems:
- OpenWrt router at `root@192.168.1.1`
- OpenWrt access point at `root@192.168.1.2`
- Proxmox host at `root@192.168.1.104`
Notable browser-managed device:
- WiCAN OBD at `http://192.168.1.165` using Chrome DevTools MCP or HTTP endpoints rather than SSH

## Working Style
- Prefer read-only inspection first, then propose or perform the smallest safe change.
- For system administration tasks, show the exact command sequence you intend to run before destructive or service-affecting actions.
- Call out risk before operations that can interrupt connectivity, reboot a host, restart networking, replace configs, or stop running workloads.
- When changing configuration files remotely, make a backup first unless the user explicitly says not to.

## Conventions
- Use SSH for remote administration unless the user asks for a web UI workflow.
- For the WiCAN device at `192.168.1.165`, prefer Chrome DevTools MCP for interactive administration and targeted HTTP requests for read-only inspection because SSH is not available.
- Treat the router and Proxmox host as separate environments with separate command sets and validation steps.
- Prefer native platform tools over generic Linux workarounds when available.
- Use the `openwrt-gateway` skill for the main router, `openwrt-ap` for the access point, and the Proxmox skill for virtualization, container, VM, storage, or cluster tasks.
- Keep the system-specific skill files current when hostnames, addresses, roles, services, mounts, or operating procedures change.
- Keep version information current in the system-specific skill files, including platform, application, package, or image-tag details when they matter operationally.
- When new Proxmox containers or VMs are created or discovered, add a dedicated guest skill for each and record the updated inventory in the workspace metadata.
- When a skill includes version context, ensure commands, paths, behavior notes, and troubleshooting guidance are relevant to that verified version; if the live version is unknown or has drifted, re-check it before giving version-specific guidance.

## Validation
- After any change, run the narrowest available verification command on the affected host.
- For networking changes, verify connectivity and the resulting active configuration.
- For container or VM changes, verify runtime state, logs, and startup behavior.