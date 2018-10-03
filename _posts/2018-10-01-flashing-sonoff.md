---
layout: post
author: phil
title: Flashing Tasmota firmware to a Sonoff Basic
categories: [iot, sonoff, firmware, tasmota]
---

Tasmota is a custom firmware replacement for the default firmware that comes with the Sonoff devices. Tasmota adds multiple features such as MQTT integration, OTA firmware updates and inbuilt timers.

## Requirements

* Sonoff Basic
* 5 pin header (socket is better but pins can be used)
* Programmer (USB to FTDI, 3.5v)
* Jumper leads
* [ESPEasy](https://github.com/letscontrolit/ESPEasy/releases), unzip and install
* [Termite](https://www.compuphase.com/software_termite.htm), unzip and install
* [sonoff.bin](https://github.com/arendst/Sonoff-Tasmota/releases), save to the same location you installed esyesp tool

## Steps

1. Pull apart your Sonoff Basic and solder your 5 pin header
2. Download and install the software listed in requirements (if you have not yet done so)
3. Wire your programmer to the Sonoff. 3.5v, Gnd, tx, rx
4. Hold down the button on the Sonoff and plug in your programemr. This will power the Sonoff but with the button held it will power it into flash mode.
5. Run the ESPEasy Tool, select the com port the programmer is connected to, select sonoff.bin from the list of firmware files.
6. Once programming has completed, cycle the power of the Sonoff, leaving the programmer connected.
7. Open Termite and connect it to the programmer com port, set the baud rate to 115700.
8. Send the following command to connect to wifi `Backlog SSID1 myWifi; Password1 password `
9. You should see the Sonoff reply with connection info.
10. You can now browse to the ip address of the Sonoff to complete the setup.


## Notes

Although you can configure everything via the web interface, you can also configure all the options via the serial interface which may help to speed up deployment.

```bash
# Setting multiple configurations options
Backlog SSID1 myWifi; Password1 myPassword; MqttHost brokerIP; MqttUser mqttUser; MqttPassword mqttPass; GPIO14 09; Hostname sonoff_name; MqttClient unique_sonoff_name; Topic sonoff_name; FriendlyName1 Sonoff-Name
```

## MQTT

By default Sonoff-Tasmota will use the following prefixes for the MQTT topic

* Prefix1 = cmnd
* Prefix2 = stat
* Prefix3 = tele

And the following topics

* Topic = sonoff_name
* GroupTopic = 
* ButtonTopic = 
* SwitchTopic = 
* MqttClient = unique_sonoff_name

### Examples

``` bash
# Switch relay on
mqtt pub -h brokerIP -u mqttUser -P mqttPass -t cmnd/sonoff_name/power -m 1

# Update configuration
mqtt pub -h brokerIP -u mqttUser -P mqttPass -t cmnd/sonoff_name/Backlog -m "ssid1 newWifi; password1 newPassword"
```
