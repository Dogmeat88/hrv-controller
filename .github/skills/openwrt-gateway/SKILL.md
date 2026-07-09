---
name: openwrt-gateway
description: 'Administer the OpenWrt router over SSH at root@192.168.1.1. Use for router configuration, firewall, DHCP, DNS, wireless, package, interface, and connectivity troubleshooting on OpenWrt.'
argument-hint: 'Describe the router task, affected service, and whether you want inspection only or a change applied.'
---

# OpenWrt Router Administration

## When to Use
- Router configuration or troubleshooting on the OpenWrt host
- Firewall, NAT, port forwarding, or policy debugging
- DHCP, DNS, or local name resolution issues
- WAN, LAN, VLAN, interface, or Wi-Fi problems
- Package and service management on OpenWrt

## Target Host
- SSH endpoint: `root@192.168.1.1`
- Platform: OpenWrt

## Version Context
- Verified 2026-06-28: OpenWrt 24.10.2 `r28739-d9340319c6`
- Apply OpenWrt guidance only when it matches the live release. Re-check firewall and service tooling if the router version changes.

## Operating Rules
- Inspect first. Do not restart networking, firewall, or wireless until the current state is captured.
- Back up any file under `/etc/config/` before changing it.
- Prefer `uci` and native OpenWrt service commands over editing generated files.
- Treat network restarts and firewall reloads as potentially disruptive and call out that risk.

## Preferred Commands
- Connect: `ssh root@192.168.1.1`
- System info: `ubus call system board`, `cat /etc/openwrt_release`, `uptime`
- Interfaces and addresses: `ip addr`, `ip route`, `ifstatus <interface>`
- Config inspection: `uci show`, `uci show network`, `uci show wireless`, `uci show firewall`, `uci show dhcp`
- Logs: `logread`, `logread -f`, `dmesg | tail`
- Services: `/etc/init.d/network status`, `/etc/init.d/firewall status`, `/etc/init.d/dnsmasq status`, `/etc/init.d/odhcpd status`
- Packages: `opkg list-installed`, `opkg update`, `opkg install <pkg>`

## Safe Change Procedure
1. Connect to the router and collect the current state with the narrowest relevant commands.
2. Save a backup before editing, for example `cp /etc/config/network /etc/config/network.bak-$(date +%F-%H%M%S)`.
3. Apply configuration with `uci`, or edit `/etc/config/*` only when that is clearer for the task.
4. Commit the specific config namespace with `uci commit <name>` when using `uci`.
5. Reload only the affected service when possible, such as `/etc/init.d/dnsmasq reload` instead of restarting all networking.
6. Validate the result with status commands and a functional check such as route lookup, DHCP lease review, or DNS resolution.

## High-Risk Actions
- `service network restart`, `/etc/init.d/network restart`, or changes to the active LAN IP can cut off SSH access.
- Firewall reloads can interrupt forwarding or management access if rules are wrong.
- Wireless changes can disconnect clients immediately.

## Validation Patterns
- Network: `ifstatus lan`, `ifstatus wan`, `ip route`, `ping -c 3 8.8.8.8`
- DNS/DHCP: `logread | grep -i dnsmasq`, `cat /tmp/dhcp.leases`
- Firewall: `nft list ruleset` if present, otherwise `iptables-save` or `fw3 print` depending on platform version
- Wireless: `iw dev`, `logread | grep -i wireless`

## Notes
- Prefer version-aware troubleshooting because OpenWrt firewall tooling differs across releases.
- If a task may drop connectivity, recommend a rollback path before applying the change.