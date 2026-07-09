# Export Meter
Display power import/export via an RGB LED strip/ring

## Prerequisites
 * [Home Assistant](https://www.home-assistant.io/)
 * Power usage feed. I.e.
   * [emonPi](https://guide.openenergymonitor.org/technical/emonpi/)
   * [ESP32 Energy Meter](https://github.com/CircuitSetup/Expandable-6-Channel-ESP32-Energy-Meter)


## Hardware
 * [ESP32-WROOM-32D](https://s.click.aliexpress.com/e/_DnBZ0rd)
 * [WS2812 Pixel Ring](https://s.click.aliexpress.com/e/_DFNlQ3H)

## Installation
[Install ESPHome](https://esphome.io/guides/installing_esphome.html)

Create a secrets.yaml file and add your Wi-Fi credentials in this format
```
wifi_ssid: '<SSID>'
wifi_password: '<PASSWORD>'
```

Then run: ```esphome run export-meter.yaml```

## Images

Exporting Power

![Export](./assets/Export01.png)

Importing Power

![Import](./assets/Import01.png)

Case Open

![Import2](./assets/Import02.png)

Case Model

![Case Parts](./assets/case-parts.png)

Home Assistant Entity

![Home Assistant Entity](./assets/HA.png)

EspHome Web

![Esphome Web](./assets/Esphome.png)

Infrastructure

![Infrastructure](./assets/infrastructure.png)

Wiring

![Wiring](./assets/wiring.png)

## Home Assistant integration - example automation
```
alias: Map Export to LED
description: ''
trigger:
  - platform: state
    entity_id:
      - sensor.emoncms_grid_consumption
condition: []
action:
  - service: number.set_value
    entity_id: number.percentage_input
    data:
      value: |
        {% set power = states('sensor.emoncms_grid_consumption')|float %}
        {% set max_power = 1500.0 %}
        {% set min_power = max_power *-1 %}
        {% if power < min_power %}
          {{ -100.0 }}
        {% elif power > max_power %}
          {{ 100.0 }}
        {% else %}
          {% if power > 0 %}
            {{ power / max_power * 100 }}
          {% else %}
            {{ power / min_power * -100 }}
          {% endif %}
        {% endif %}
mode: ''
```