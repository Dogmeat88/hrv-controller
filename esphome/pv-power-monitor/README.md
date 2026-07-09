# PV Power Monitor
Monitor DC Voltage and Power 

## Hardware
* [ESP32-WROOM-32D Microcontroller](https://s.click.aliexpress.com/e/_DnBZ0rd)

## Installation

Install [ESPHome](https://esphome.io/guides/installing_esphome.html)

Create a ```secrets.yaml``` file and add your Wi-Fi credentials in this format

````
wifi_ssid: '<SSID>'
wifi_password: '<PASSWORD>'
````

Then run: ```esphome run pv-power-monitor.yaml```

Optionally this can also be integrated with [Home Assistant](https://www.home-assistant.io/) to provide nicer UI and automations etc 


