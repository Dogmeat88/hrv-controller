---
name: proxmox-vm-104-haos
description: 'Administer Proxmox VM 104 haos-17.3 on root@192.168.1.104. Use Home Assistant MCP as the default way to interact with Home Assistant; use this skill for HAOS VM health, guest agent checks, VM config, and host-side troubleshooting.'
argument-hint: 'Describe the Home Assistant task. Use Home Assistant MCP by default for entity, automation, script, and dashboard work; use host-side diagnostics for VM or HAOS health issues.'
---

# Proxmox VM 104 Home Assistant OS

## Target
- Host: `root@192.168.1.104`
- Guest type: VM `104`
- Name: `haos-17.3`
- Guest OS: Home Assistant OS 18.0
- Config: 2 cores, 4 GB RAM, 100 GB disk, guest agent enabled
- Observed guest IP: `192.168.1.107`

## Version Context
- Verified 2026-06-28: Home Assistant OS 18.0, kernel `6.18.35-haos`
- Use Home Assistant MCP guidance only when it matches the live Home Assistant and HAOS behavior. Re-check versions before giving version-specific integration or recovery advice.

## Use When
- Interacting with Home Assistant through Home Assistant MCP for normal Home Assistant tasks
- Troubleshooting Home Assistant OS startup, networking, storage, or VM config
- Checking guest-agent connectivity or host-side VM health

## Default Interaction Mode
- Use Home Assistant MCP as the default way to interact with Home Assistant.
- Prefer Home Assistant MCP for entities, states, automations, scripts, dashboards, integrations, and routine diagnostics inside Home Assistant.
- Use Proxmox host-side commands only when the issue is with VM power state, guest-agent availability, storage, networking, or Home Assistant OS boot health.

## Preferred Commands
- Status: `ssh root@192.168.1.104 'qm status 104 && qm config 104'`
- Guest agent: `ssh root@192.168.1.104 'qm agent 104 ping && qm agent 104 get-osinfo'`
- Network info: `ssh root@192.168.1.104 'qm guest cmd 104 network-get-interfaces'`
- Console: `ssh root@192.168.1.104 'qm terminal 104'`

## When To Stay In MCP
- Inspecting entity state, device behavior, automation runs, scripts, helpers, dashboards, or integration behavior
- Triggering or validating Home Assistant actions that do not require VM-level recovery

## When To Use Proxmox Or HAOS Diagnostics
- Home Assistant is unreachable or not booting
- The VM is stopped, hung, or missing guest-agent responses
- The issue may involve VM disk, CPU, RAM, network attachment, or host-side failures

## Validation
- Verify VM `104` is running and the guest agent responds.
- Confirm the expected guest IP and host-side VM configuration after changes.
- For normal Home Assistant work, validate through Home Assistant MCP first.
- For infrastructure issues, validate host-side VM health first, then continue with HA-supported recovery paths.