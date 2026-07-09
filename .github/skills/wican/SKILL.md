---
name: wican
description: 'Administer the WiCAN OBD device at http://wican_f0f5bdf7e2dd.local using Chrome DevTools MCP or targeted HTTP requests. Use for status checks, CAN/MQTT configuration review, browser UI troubleshooting, configuration backup or restore, and firmware maintenance.'
argument-hint: 'Describe the WiCAN task, such as checking status, reviewing CAN or MQTT settings, backing up config, updating firmware, or troubleshooting browser access.'
---

# WiCAN OBD Administration

## Target
- Device: WiCAN OBD
- Web UI: `http://wican_f0f5bdf7e2dd.local`
- mDNS: `wican_f0f5bdf7e2dd.local`
- Current observed station IP: `192.168.1.167`
- Access method: browser UI via Chrome DevTools MCP; no SSH access

## Version Context
- Verified 2026-07-06 from live `/check_status`: firmware `4.20`, software build `v4.20_beta-01`, hardware `WiCAN-OBD`
- Current observed runtime on 2026-07-09: WiFi mode `AP+Station`, station IP `192.168.1.167`, CAN bitrate `500K`, CAN mode `Normal`, transport `TCP` on port `3333`, protocol `slcan`, MQTT `enabled`, power saving `enabled`
- Re-check `/check_status` before giving version-specific guidance after any firmware change.

## Use When
- Troubleshooting reachability to the WiCAN web UI
- Checking live WiFi, CAN, MQTT, battery, or uptime status
- Reviewing or changing WiCAN settings in the browser UI
- Backing up or restoring configuration
- Performing or validating a firmware update

## Operating Rules
- Prefer read-only inspection first with `curl` or the browser UI.
- Use Chrome DevTools MCP for interactive UI tasks because the device is browser-managed.
- Treat `GET /check_status` as sensitive: it exposes live configuration values and may include WiFi or MQTT credentials. Do not paste full responses into notes or commits.
- Call out risk before rebooting the device, uploading configuration, or starting a firmware update.
- Back up the current configuration before firmware updates or larger settings changes.

## Preferred Commands
- Reachability: `ping -c 1 wican_f0f5bdf7e2dd.local` or `ping -c 1 192.168.1.167`
- UI fetch: `curl -s http://wican_f0f5bdf7e2dd.local/ | head`
- Live status (redacted): `curl -s http://wican_f0f5bdf7e2dd.local/check_status | jq '{fw_version, git_version, hw_version, wifi_mode, sta_connected, sta_status, sta_ip, can_mode, can_datarate, port_type, port, protocol, mqtt_en, sleep_status, sleep_time, sleep_volt, wakeup_time, wakeup_volt, ap_auto_disable, batt_voltage, timestamp, uptime}'`
- Browser workflow: open `http://wican_f0f5bdf7e2dd.local` with Chrome DevTools MCP, inspect `Status`, `Settings`, `Automate`, `Monitor`, `Power Saving`, `System`, and `About`
- Firmware update path: `POST` upload to `/upload/ota.bin` from the `System` tab

## Validation
- Confirm the device responds to `ping` and the web UI loads.
- After any change, re-check `GET /check_status` and verify the specific fields that were intended to change.
- After configuration restore or firmware update, verify browser access, CAN mode, transport port, and runtime status.
- After a reboot, confirm the device returns via `wican_f0f5bdf7e2dd.local` and note the current station IP if DHCP has changed it.