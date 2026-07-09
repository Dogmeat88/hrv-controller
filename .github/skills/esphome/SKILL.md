---
name: esphome
description: 'Work with ESPHome device configurations in this repository. Use for ESPHome config review, migration to current schema, compile or validation checks, OTA workflows, secrets handling, Home Assistant cross-reference, and reusable ESPHome conventions. Prefer the official ESPHome documentation for component syntax and validation behavior.'
argument-hint: 'Describe the ESPHome device, YAML file, and whether the goal is review, migration, validation, compile, OTA upload, or Home Assistant alignment.'
---

# ESPHome Configuration Workflow

## When to Use
- Creating, reviewing, or editing ESPHome YAML configurations
- Migrating older ESPHome configs to current syntax
- Validating or compiling nodes with the local ESPHome CLI
- Uploading firmware over OTA
- Consolidating or reusing secrets across multiple ESPHome devices
- Cross-checking ESPHome devices against Home Assistant usage

## Workspace Scope
- Repository root: `/home/i/repos/SysAdmin`
- Consolidated ESPHome tree: `/home/i/repos/SysAdmin/esphome`
- Shared secrets file: `/home/i/repos/SysAdmin/esphome/secrets.yaml`
- Shared secrets template: `/home/i/repos/SysAdmin/esphome/secrets.example.yaml`
- Local CLI: `esphome`
- Installed CLI version last verified in this workspace: `2026.6.5`

## Repository Conventions
- Each device lives in its own directory under `esphome/`.
- Each device directory should contain one top-level YAML entrypoint with the same name as the directory.
- Keep only the config file, README when useful, and the local `secrets.yaml` symlink unless another file is clearly needed.
- Each device directory needs a local `secrets.yaml` symlink pointing to `../secrets.yaml` so `esphome config` resolves shared secrets correctly.
- Shared Wi-Fi secrets use `wifi_ssid` and `wifi_password` from the central secrets file.
- Validate the specific node after every change with the narrowest possible ESPHome command.

## Preferred Workflow
1. Read the target YAML and stay local to the controlling component or failing section.
2. Check the current ESPHome docs before guessing component syntax or option names.
3. Make the smallest viable change that addresses the root cause.
4. Run `esphome config <device>/<device>.yaml`.
5. Run `esphome compile <device>/<device>.yaml` when the change affects generated code, schema, or component interactions.
6. For deployment changes, verify reachability first, then use `esphome upload <device>/<device>.yaml --device <host>`.

## Preferred Commands
- Validate config: `cd /home/i/repos/SysAdmin/esphome && esphome config <device>/<device>.yaml`
- Compile firmware: `cd /home/i/repos/SysAdmin/esphome && esphome compile <device>/<device>.yaml`
- Upload over OTA: `cd /home/i/repos/SysAdmin/esphome && esphome upload <device>/<device>.yaml --device <hostname-or-ip>`
- Clean build artifacts for a node when necessary: `cd /home/i/repos/SysAdmin/esphome && rm -rf <device>/.esphome/build`

## Common Migration Patterns
- Replace legacy `platform` declarations with current `esp32:` or `esp8266:` blocks.
- Use list-form `ota:` with `- platform: esphome`.
- Replace deprecated `dallas` usage with `one_wire` plus `dallas_temp`.
- Replace obsolete code patterns like `ESPColor(...)` or old sensor accessors with the current API.
- Re-check older custom lambdas against the installed ESPHome version before editing.

## Home Assistant Alignment
- Prefer importing only devices that are still active or intentionally retained.
- When useful, cross-reference entities in Home Assistant before carrying forward stale configs.
- For operational debugging, expose template sensors only when they help tune or validate real behavior.

## Safe Change Rules
- Do not change shared secrets structure casually; many device configs depend on it.
- Treat OTA uploads as service-affecting because the device will reboot.
- Check network reachability before OTA upload.
- Do not remove deprecated or broken configs blindly if they may still represent production hardware; confirm current use first.

## Validation Patterns
- Schema-only check: `esphome config <device>/<device>.yaml`
- Full firmware generation: `esphome compile <device>/<device>.yaml`
- OTA readiness: `ping -c 1 -W 2 <hostname>` and resolve the node host first if needed
- Post-upload confirmation: watch the ESPHome upload result and verify the device reconnects in Home Assistant

## Official References
- Main docs: `https://esphome.io/`
- Components index: `https://esphome.io/components/`
- CLI guide: `https://esphome.io/guides/getting_started_command_line.html`
- Configuration types and substitutions: `https://esphome.io/guides/configuration-types.html`
- Automations and lambdas: `https://esphome.io/automations/actions`
- Template entities: `https://esphome.io/components/sensor/template`

## Local References
- Consolidated ESPHome overview: `/home/i/repos/SysAdmin/esphome/README.md`
- Shared secrets template: `/home/i/repos/SysAdmin/esphome/secrets.example.yaml`
- Example advanced node in this repo: `/home/i/repos/SysAdmin/esphome/hrv-controller/hrv-controller.yaml`

## Notes
- Some compile failures are network or dependency fetch issues rather than config defects; retry once before changing code if the error points to package download failure.
- Editor YAML warnings for `!secret` may be noise if the per-device `secrets.yaml` symlink is present and `esphome config` passes.