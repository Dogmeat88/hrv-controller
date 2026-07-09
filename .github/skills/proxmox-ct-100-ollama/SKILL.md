---
name: proxmox-ct-100-ollama
description: 'Administer Proxmox CT 100 ollama on root@192.168.1.104. Use for Ollama service health, model serving, logs, resource checks, and LXC troubleshooting.'
argument-hint: 'Describe the Ollama issue or change needed, such as model serving, startup, logs, storage, or resource tuning.'
---

# Proxmox CT 100 Ollama

## Target
- Host: `root@192.168.1.104`
- Guest type: LXC container `100`
- Hostname: `ollama`
- OS: Ubuntu 24.04.4 LTS
- Config: 4 cores, 24 GB RAM, 110 GB rootfs, 2 GB swap, DHCP on `vmbr0`, unprivileged LXC
- GPU passthrough: NVIDIA GeForce RTX 3060 exposed as `/dev/nvidia*`

## Version Context
- Verified 2026-06-29: Ubuntu 24.04.4 LTS, Ollama 0.30.6, NVIDIA driver 595.71.05 / CUDA 13.2
- Use Ollama-specific guidance only when the live API or CLI version still matches. Re-check model-management and API behavior after upgrades.
- In `pct exec 100 -- bash -lc ...` contexts, `ollama` may not be on `PATH`; prefer `/usr/local/bin/ollama`.
- Current local tuning: `optimized-35b:latest` uses `num_gpu 12` and `num_ctx 2048`; prior profiles are preserved as `optimized-35b-14:latest` and `optimized-35b-15:latest`.

## Use When
- Troubleshooting Ollama availability, performance, or logs
- Managing models, disk usage, or service restarts inside the Ollama container

## Preferred Commands
- Status: `ssh root@192.168.1.104 'pct status 100 && pct exec 100 -- systemctl status ollama --no-pager'`
- Logs: `ssh root@192.168.1.104 'pct exec 100 -- journalctl -u ollama -n 200 --no-pager'`
- Runtime: `ssh root@192.168.1.104 'pct exec 100 -- /usr/local/bin/ollama ps && pct exec 100 -- /usr/local/bin/ollama list'`
- Shell: `ssh root@192.168.1.104 'pct enter 100'`
- Service restart: `ssh root@192.168.1.104 'pct exec 100 -- systemctl restart ollama'`

## Validation
- Verify the container is running and the `ollama` unit is active.
- Check recent `journalctl` output after changes.
- Confirm model-serving behavior with the narrowest relevant client request.
- If GPU offload is relevant, confirm backend selection from logs (`CUDA0` vs CPU) and compare `/usr/local/bin/ollama ps` with `nvidia-smi`.