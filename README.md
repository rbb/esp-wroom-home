# ESPHOME With a ESP32-WROOM Dev Kit

I bought some ESP32-WROOM Dev Kits from [amazon](https://www.amazon.com/dp/B08D5ZD528).
This is a project to track the code I've used, and the progress I've made, using
these boards with [ESPHome](https://esphome.io).

# General Notes

* Mac OS Big Sur (and presumably later) should have built in support for the
  USB virtual serial port chip (CPxxxx) builtin. No need for the Silabs CP210x
  VCP Driver!

## Programming The ESP32 WROOM:
   * I couldn't get the boards to show up on WiFi, via my home assistant
     instance, with the ESPhome plugin installed.
   * I don't have Chrome or MS Edge installed on my Mac, so to program over USB,
     I had to install esphome via Python/pip (`pip3 install wheel esphome`).
   * After trying a bunch of USB cables, I was finally able to find one with
     data lines. This seems to be a common problem. The syptom is that without
     the data lines, I don't see it show up with `lsusb` (linux or homebrew on
     mac) or `system_profiler SPUSBDataType`, which is essentially the same as
     Apple menu -> About This Mac -> System Report -> Hardware -> USB, which is
     similar to using the Device Manager in Windows.
   * Once programmed, and connected to Home Assistant, a power only UBS cable
     can be used.

So my bring up process looks like:

1. Edit the yaml code:
   1. In the locally running dashboard `esphome dashboard .`
   2. In the dashboard of my ESPHome plugin in my HomeAssistant instance
   3. With my favorite text editor (vim)
2. Compile with `esphome compile file.yaml`
3. Load it into the ESP32 with `esphome compile file.yaml`, then selecting the
USB serial port option.
4. (Optional) check the logs with `esphome logs file.yaml`
5. Use the Home Assistant GUI to include entities in dashboards and automations.

# Examples

1. Loggin for the Airthings Wave Mini (see `airthings2.yaml`)
2. As an Analog Sensor

## LED

Follow the [binary light](https://esphome.io/components/light/binary) example
to toggle the Blue LED on the board. On the board I ordered, the pin is GPIO2.
I've found this useful, as a basic 'does it work' thing. I expect it will be
useful when identifying multiple dev boards.

## Airthings Logger

See the `airthings2.yaml` for an example. This example is based on the [ESPHome
AirThings BLE Sensors](https://esphome.io/components/sensor/airthings_ble.html).

This example includes the LED toggle via home assistant.

With this yaml, board bring up process becomes:
1. Edit the yaml (without the BLE MAC address of the airthings)
2. compile the yaml
3. load the compiled yaml
4. check the logs (`esphome logs airthings2.yaml`)
5. Copy/Paste the BLE MAC address from the logs to the `airthings2.yaml` file
6. Re-compile yaml
7. Load the newly compiled yaml
8. Check the logs again, to make sure the MAC Address took, and we're getting
readings.
9. Use the Home Assistant GUI to do things with the new entities.

## Door Sensor

See the `garagedoor.yaml` for an example.

In this example I use the LED on the ESP32 as an indicator of when the door
is open (LED on) or closed (LED off).

For the sensor itself, I used "Surface Mount Alarm, Door Window", magnetic reed
switches from [amazon](https://www.amazon.com/gp/product/B07F314V3Z). I wired
one end to GPIO5, which hass a pull-up built in to the EPS32, and the other end
to GND. Doesn't matter which end of the sensor goes to which pin.

## Analog Sensor

Note: Despite claiming to have a built-in temperature sensor in the Amazon
description, the info on [espboards.dev](https://www.espboards.dev/blog/esp32-inbuilt-temperature-sensor/)
indicates that the ESP32-WROOM does not have a built in temperature sensor.

# References

* [ESP Home Airthings BLE](https://esphome.io/components/sensor/airthings_ble.html)
* [Schematics](https://github.com/SmartArduino/ESPboard/blob/master/esp32manual.rar)

# TODO

1. See if MQTT is worth enabling to get the logs
