# HRV controller
This is a smart replacement for an HRV/ERV controller

The software will automatically switch between heating and cooling mode depending on the attic and house temperature comparison.

Setting the mode to 'off' will override the automatic mode setting. 

**Disclosure:** This README contains affiliate links. I may earn a commission from qualifying purchases. #Ad

## Hardware
* [ESP32-WROOM-32D Microcontroller](https://s.click.aliexpress.com/e/_DnBZ0rd)
* [DS18B20 Temperature Sensor x2](https://s.click.aliexpress.com/e/_DlF2SIx)
* [RobotDyn Dimmer/Motor Controller module](https://s.click.aliexpress.com/e/_DlDin6n)
* [Mains to 5v DC adapter](https://s.click.aliexpress.com/e/_DBtTMj5)


## Installation

Install [ESPHome](https://esphome.io/guides/installing_esphome.html)

Create a ```secrets.yaml``` file and add your Wi-Fi credentials in this format

````
wifi_ssid: '<SSID>'
wifi_password: '<PASSWORD>'
````

Then run: ```esphome run hrv-controller.yaml```

## Sync workflow

This directory is mirrored from the standalone repo at `/home/i/repos/hrv-controller` through a git subtree in the SysAdmin repository.

Special subtree commands from `/home/i/repos/SysAdmin`:

- Pull upstream HRV repo changes into SysAdmin: `git subtree pull --prefix=esphome/hrv-controller /home/i/repos/hrv-controller master --squash`
- Push SysAdmin subtree changes back to the standalone HRV repo: `git subtree push --prefix=esphome/hrv-controller /home/i/repos/hrv-controller master`
- Recreate the local secrets link after subtree operations if needed: `ln -sfn ../secrets.yaml esphome/hrv-controller/secrets.yaml`

Optionally this can also be integrated with [Home Assistant](https://www.home-assistant.io/) to provide nicer UI and automations etc 

## Display
![](assets/hrv-display.png)

## Circuit Diagram
![](assets/hrv-controller-circuit.png)

## Assembly
![](assets/hrv-controller-assembly.png)

