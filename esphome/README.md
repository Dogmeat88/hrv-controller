# ESPHome Configs

This directory consolidates the ESPHome configurations from `/home/i/repos/git/ESPHome` that still map to live ESPHome devices in Home Assistant.

Entrypoint convention: each config directory uses a single top-level YAML file whose name matches the directory name.

Cleanup convention: generated local build/cache directories such as `.esphome/`, `.pioenvs/`, and `.piolibdeps/` are not kept here. Source-support files such as `platformio.ini`, `partitions.csv`, `src/`, and `lib/` are kept when they are part of the hand-maintained project.

Secrets convention: all imported configs use the shared top-level `secrets.yaml` in this directory with a single `wifi_ssid` and `wifi_password`. Keep the real file untracked and use `secrets.example.yaml` as the tracked template.

Imported configs:

- `6ch-power`
- `export-diversion-controller`
- `export-meter`
- `ha-voice`
- `hrv-controller`
- `pv-hwc-monitor`
- `pv-power-monitor`
- `sonoff-pow-r2`
- `sonoff-switch-t1`
- `still-monitoring`
- `usb-relay`

Not imported, no current Home Assistant ESPHome device match found:

- `ARCHIVE`
- `auto-brake-light`
- `environment-sensor`
- `hrv-panel`
- `led-show`
- `picow`
- `pv-meter`
- `sonoff-dual-r3`

Not imported, appears superseded by another active device or non-ESPHome integration:

- `6ch-powerx` alternate of `6ch-power`
- `presence` matched a ZHA device, not an ESPHome device
- `sonoff-basic-r2`
- `sonoff-plug`
- `sonoff-pow-r3`
- `sonoff-th16`
- `temperature-sensor` matched a non-ESPHome temperature sensor in Home Assistant

`secrets.yaml` files are intentionally excluded from version control in this repository.

The duplicate `6chan_energy_meter` configs were resolved in favor of `6ch-power`, which appears to be the live variant based on its newer local edits and the currently active Home Assistant device.

After import, inconsistent entrypoint filenames were normalized to this repository convention:

- `export-diversion-controller/diversion.yaml` -> `export-diversion-controller/export-diversion-controller.yaml`
- `hrv-controller/hrv.yaml` -> `hrv-controller/hrv-controller.yaml`
- `pv-hwc-monitor/dc-meter.yaml` -> `pv-hwc-monitor/pv-hwc-monitor.yaml`
- `still-monitoring/ESP32CAM-Distill.yaml` -> `still-monitoring/still-monitoring.yaml`