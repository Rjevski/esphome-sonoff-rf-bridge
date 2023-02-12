# ESPHome config for Sonoff RF Bridge

This provides a configuration for the Sonoff RF Bridge.

## Installation

Adjust the ESPHome config as required (replace secrets, etc) then compile it. Set the resulting image aside for now, you'll need it in the steps below.

### Original RF firmware

This option directly installs ESPHome while keeping the stock firmware on the separate RF-related MCU.

Follow the [Tasmota](https://tasmota.github.io/docs/devices/Sonoff-RF-Bridge-433/) instructions but use your ESPHome image instead of the Tasmota image.


### Improved open-source RF firmware

This option installs the [Portisch](https://github.com/Portisch/RF-Bridge-EFM8BB1) firmware on the RF MCU which has more advanced features.

Follow the [Tasmota](https://tasmota.github.io/docs/devices/Sonoff-RF-Bridge-433/) instructions, **install Tasmota**, then follow the "RF Firmware Upgrade" steps (use Tasmota to update to the Portisch firmware).

Once the RF firmware has been updated, try to use the Tasmota web UI to do an OTA upgrade using your ESPHome image. If that fails, install it via the hardware by following the initial Tasmota installation steps but this time using the ESPHome image instead of Tasmota.

## Controllable logo backlight mod

By default the logo backlight is connected through a resistor to the power rail. Conveniently, a GPIO (test?) pad on the board is within reach of said resistor, so it can just be desoldered from its original place and soldered to the GPIO pad (may need to shorten the lead slightly to make it neat). If you've done this, you can uncomment the relevant part in `esphome.yaml` and a dimmable Light entity will be exposed to Home Assistant which you can control. You can of course extend the configuration further and use this light object for something else (status light, etc).


## Usage with Home Assistant

This configuration will emit events like `esphome.rf_code_received` which you can use to trigger automations.

### Door/window sensors

For door/window sensors we need to expose them as entities in Home Assistant - you can use [trigger-based template sensors](https://www.home-assistant.io/integrations/template/#trigger-based-template-binary-sensors-buttons-numbers-selects-and-sensors) for that. See an example in `homeassistant.yaml`. Make sure to replace the `event_data.code` with your own, which you can get by listening to the `esphome.rf_code_received` in Home Assistant developer tools and actuating your sensor.