---
name: proxmox-ct-108-litellm-proxy
description: 'Administer Proxmox CT 108 litellm-proxy on root@192.168.1.104. Use for LiteLLM proxy service health, routing, logs, and LXC troubleshooting.'
argument-hint: 'Describe the LiteLLM proxy issue, such as startup, provider routing, auth, logs, or configuration.'
---

# Proxmox CT 108 LiteLLM Proxy

## Target
- Host: `root@192.168.1.104`
- Guest type: LXC container `108`
- Hostname: `litellm-proxy`
- OS: Debian GNU/Linux 13
- Config: 2 cores, 2 GB RAM, 10 GB rootfs, DHCP on `vmbr0`, firewall enabled on `net0`

## Version Context
- Verified 2026-06-28: Debian GNU/Linux 13, LiteLLM `1.88.1`
- Keep provider-routing, config, and CLI guidance aligned to the live LiteLLM version. Re-check options and config schema after upgrades.
- Current config path: `/opt/litellm/config.yaml`
- Current routes: `qwen2.5-coder:14b` -> `ollama_chat/qwen2.5-coder:14b`, `optimized-35b:latest` -> `ollama_chat/optimized-35b:latest`
- Current behavior note: `optimized-35b:latest` is configured with `extra_body.think: false` and `options.num_ctx: 2048` to avoid empty `content` responses from long reasoning-only generations.

## Use When
- Troubleshooting LiteLLM proxy startup, upstream routing, or service logs
- Inspecting the container when requests fail or provider connectivity is broken

## Preferred Commands
- Status: `ssh root@192.168.1.104 'pct status 108 && pct exec 108 -- systemctl status litellm --no-pager'`
- Logs: `ssh root@192.168.1.104 'pct exec 108 -- journalctl -u litellm -n 200 --no-pager'`
- Shell: `ssh root@192.168.1.104 'pct enter 108'`
- Failed units: `ssh root@192.168.1.104 'pct exec 108 -- systemctl --failed --no-pager'`

## Validation
- Verify CT `108` is running and `litellm` is active.
- Check logs for upstream provider, auth, or bind errors.
- Re-test the specific proxy route or model call that was failing.