# ESPHome-DS18B20-My_Way

Tailoring [SPHome-foundation](https://github.com/HankB/ESPHome-foundation) for my specific uses. All of my other sensors feed Home Assistant (HA) by publishing to an MQTT broker to which HA subscribes for topics of interest. To follow that pattern I have disabled the HA integration in this code and instead publish.

## Motivation and purpose

Previous ESP8266/ESP32 projects have suffered from issues. In particular <https://github.com/HankB/ESP32-ESP-IDF-CLI-DS18B20> runs for a while and just stops publishing.

## 2026-05-08 Plan

Eliminate as much of the extraneous MQTT traffic as possible/necessary. The template was not tested with HA so I'll do that once I get a properly crafted message for my application. That will look like:

```text
HA/esp32-b9ace8/10g_tank/temperature {"t":1778249810,"temp":73.175,"device":"DS18B20","DS18B20_ID":17509995352672987176}
```

## Status

* 2026-05-07 Copied and README updated.

## 2026-05-06 Setup

Initial install in a Python virtual environment is:

```text
python3 --version
pushd ~/esp
python3 -m venv venv
source venv/bin/activate
pip3 install esphome
esphome version
```

```text
(venv) hbarta@olive:~/esp$ esphome version
Version: 2026.4.5
(venv) hbarta@olive:~/esp$ 
```

ESPHome has been installed in `~/ESP/venv` and can be started by typing:

```text
source ~/esp/venv/bin/activate
esphome version
```

## 2026-05-06 build and flash

```text
esphome wizard ESPHome-DS18B20-My_Way.yaml  # first run - not used since this was copied from ESPHome-foundation
esphome run ESPHome-DS18B20-My_Way.yaml     # Subsequent runs
esphome logs ESPHome-DS18B20-My_Way.yaml    # Monitor w/out flashing target
esphome compile ESPHome-DS18B20-My_Way.yaml # build w/out flashing
```

## Errata

* 2026-05-07 ESPHome seems not to provide a templating option for setting hostname. For example it would be useful to have something like "[family]-[MAC-3]" that wouild expand to something like `ESP32-B9ACE8` where the device's MAC address is `08:3A:F2:B9:AC:E8`. there is a "substitute capacity" that allows to define the name once and use it myltiple times.
* 2026-05-07 Default MQTT setup publishes the following messages, the last of which suggests it sets a last will and testament.

```text
esp32-b9ace8/status online
esphome/discover/esp32-b9ace8 {"ip":"10.20.1.85","name":"esp32-b9ace8","version":"2026.4.5","mac":"083af2b9ace8","platform":"ESP32","board":"esp32dev","network":"wifi"}
esp32-b9ace8/status offline
```

* 2026-05-07 while monitoring the ESP32 appeared to reboot. This should not be due to the warning about 15 minute reboot at <https://esphome.io/components/mqtt/>