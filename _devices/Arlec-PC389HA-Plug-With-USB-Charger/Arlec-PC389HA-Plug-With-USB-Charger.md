---
title: Arlec PC389HA Plug With 2.1A USB Charger
date-published: 2020-09-10
type: plug
standard: au
---
1. TOC
{:toc}

## GPIO Pinout

| Pin     | Function                           |
|---------|------------------------------------|
| GPIO4   | Blue LED (Inverted: true)          |
| GPIO12  | Relay                              |
| GPIO13  | Red LED (Inverted: true)           |
| GPIO14  | Button (Inverted: true)            |

## Basic Configuration

```yaml
substitutions:
  item_name: "arlec_pc389ha_001"
  friendly_name: "Fairy Lights"

esphome:
  name: ${item_name}
  comment: ${friendly_name}
  platform: ESP8266
  board: esp01_1m
  
wifi:
  ssid: 'ssid'
  password: 'password'
  
logger:

api:
  password: 'api_password'

ota:
  password: 'ota_password'
  
binary_sensor:
  - platform: gpio
    pin:
      number: 14
      mode: INPUT_PULLUP
      inverted: true
    name: "${friendly_name} Button State"
    internal: true
    on_press:
      - switch.toggle: relay

  - platform: status
    name: "${friendly_name} Status"

switch:
  - platform: gpio
    id: blue_led
    pin:
      number: GPIO4
      inverted: true

  - platform: gpio
    id: red_led
    pin:
      number: GPIO13
      inverted: true

  - platform: gpio
    name: "${friendly_name}"
    pin: GPIO12
    id: relay
    on_turn_on:
      # Turn off blue LED to show blue when turned on
      - switch.turn_off: red_led
      - switch.turn_on: blue_led
    on_turn_off:
      # Turns on the red LED once the plug is turned off
      - switch.turn_off: blue_led
      - switch.turn_on: red_led

sensor:
  - platform: uptime
    name: ${friendly_name} Uptime
    
  - platform: wifi_signal
    name: "${friendly_name} WiFi Signal"
    update_interval: 60s
```
