---
name: openwrt-ap
description: 'Administer the OpenWrt access point over SSH at root@192.168.1.2. Use for AP configuration, wireless, bridge, LAN, DHCP disablement, package management, and connectivity troubleshooting on OpenWrt.'
argument-hint: 'Describe the AP task, affected wireless or LAN component, and whether you want inspection only or a change applied.'
---

# OpenWrt Access Point Administration

## When to Use
- Access point configuration or troubleshooting on the OpenWrt AP host
- Wireless SSID, channel, encryption, roaming, or client connectivity issues
- LAN bridge, management IP, or uplink troubleshooting on the AP
- Validation that DHCP, firewall, and routing behavior remain appropriate for AP mode
- Package and service management on the OpenWrt AP

## Target Host
- SSH endpoint: `root@192.168.1.2`
- Platform: OpenWrt
- Role: access point on the main LAN

## Version Context
- Verified 2026-06-28: OpenWrt 24.10.2 `r28739-d9340319c6`
- Keep AP-mode guidance aligned to the live OpenWrt release. Re-check bridge, wireless, and service behavior if the device version changes.

## Operating Rules
- Inspect first. Do not restart networking or wireless until the current state is captured.
- Back up any file under `/etc/config/` before changing it.
- Prefer `uci` and native OpenWrt service commands over editing generated files.
- Treat wireless reloads, bridge changes, and management IP changes as potentially disruptive and call out that risk.
- Preserve AP-mode assumptions unless the user explicitly asks otherwise: typically no WAN routing role, DHCP server disabled, and LAN bridged to wireless.

## Preferred Commands
- Connect: `ssh root@192.168.1.2`
- System info: `ubus call system board`, `cat /etc/openwrt_release`, `uptime`
- Interfaces and addresses: `ip addr`, `ip route`, `ifstatus lan`, `brctl show` or `bridge link`
- Config inspection: `uci show`, `uci show network`, `uci show wireless`, `uci show dhcp`, `uci show firewall`
- Logs: `logread`, `logread -f`, `dmesg | tail`
- Services: `/etc/init.d/network status`, `/etc/init.d/dnsmasq status`, `/etc/init.d/firewall status`, `/etc/init.d/odhcpd status`
- Wireless: `iw dev`, `iwinfo`, `wifi status`

## Safe Change Procedure
1. Connect to the AP and collect the current state with the narrowest relevant commands.
2. Save a backup before editing, for example `cp /etc/config/wireless /etc/config/wireless.bak-$(date +%F-%H%M%S)`.
3. Apply configuration with `uci`, or edit `/etc/config/*` only when that is clearer for the task.
4. Commit only the specific config namespace with `uci commit <name>`.
5. Reload only the affected service when possible, such as `wifi reload` or `/etc/init.d/network reload`.
6. Validate the result with status commands and a functional check such as client association, bridge membership, or management-IP reachability.

## High-Risk Actions
- Changing the AP management IP can cut off SSH access.
- Wireless channel, SSID, or encryption changes can disconnect clients immediately.
- Bridge or VLAN changes can isolate the AP from the main LAN.
- Enabling DHCP or routing accidentally can create conflicts with the main gateway.

## Validation Patterns
- Wireless: `iw dev`, `iwinfo`, `logread | grep -i wireless`
- LAN bridge: `bridge link`, `bridge vlan show`, `ifstatus lan`
- AP-mode checks: `uci show dhcp`, `uci show firewall`, `ip route`
- Connectivity: `ping -c 3 192.168.1.1`, `ping -c 3 8.8.8.8` when upstream access should exist

## Notes
- Keep AP-mode configuration aligned with the main router at `192.168.1.1` unless the user asks to diverge.
- If SSH trust is not established on the local machine, the first connection may require accepting the host key manually.