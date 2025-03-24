## Firmware

The whole Magic Clock system is controlled by an ESP32 Wroom board with an [esphome](https://esphome.io/) firmware. It offers many features out of the box that we need:

- Servo Control
- LED / Switch Controls
- Home Assistant API support
- Wifi
- Wifi configuration in your webbrowser
- VPN

esphome takes a configuratuon file for your project and compiles a firmware for several microcontroller platforms. The configuration file for the Magic Clock is [magic-clock.yaml](magic-clock.yaml). You may need to adjust the file from this repo to your project.

### Setup

esphome can either use stores credentials to connect to a Wifi network, or, if no such network can be found, will open a configuration wifi network that you can connect to with your phone and set up your wifi. An optional password can be set in the configuration. Default configuration for the setup-wifi is ssid _Magic-Clock Setup_ with password _Foxtrott1308_

Current configuration has two slots for pre-installed wifi networks. Actual credentials will be read from a secrets.yaml file

- [Wifi configuration documentation](https://esphome.io/components/wifi.html)
- [Captive Portal documentation](https://esphome.io/components/captive_portal.html)

#### Secrets.yaml

Configuration and credentials should be kept separate. The included configuration will pull wifi and other credentials from a secrets.yaml file in the same directory as the configuration file. You can use the provided example to create your own secrets.yaml-file.

#### Home Assistant and alternatives

esphome integrates natively with Home Assistant or ioBroker.

[Home Assistant API Documentation](https://esphome.io/components/api.html) This link will also generate a random key for you that you can include in your secrets.yaml to encrypt communication between Home Assistant and the Magic Clock. Home Assistant will ask for this key, too when adding it as a device.

If you want to use some a different software stack you can also configure esphome to include a webservice or [MQTT](https://esphome.io/components/mqtt.html) connection to accept commands to set the dial to any given position.

#### Hardware configuration

As you will probably use different pins then my build, you will need to edit the configuration to match your build. You can find the documentation for those configuration sections here:

- [Status Led](https://esphome.io/components/status_led.html)
- [Switch](https://esphome.io/components/switch/) LED hardware configuration. Will be exposed to Home Assistant
- [Binary Sensor](https://esphome.io/components/binary_sensor/) Pushbuttons. Will be exposed to Home Assistant
- [Output](https://esphome.io/components/output/#output) and [Servo](https://esphome.io/components/servo.html)
- [Number](https://esphome.io/components/number/) Will be exposed to Home Assistant. Includes the logic to control the servo motor (using esphome Servo component) when this value is changed in Home Assistant.

### VPN

In my setup, the Magic Clock will not be in the same network as my Home Assistant Server. (Actually, it will be on a different continent) As Home Assistant will use the esphome integration to connect to the Magic Clock (and not the other way round), the Clock needs to be reachable for your Home Assistant server in your home network.

To solve that problem, esphome includes a client for the wireguard vpn.

My router supports giving access to my local network to external devices using the wireguard protocol, too, so I set up wireguard VPN access to my home network. The credentials and settings that I have been provided here go to the _wg_ section of the configuration file. (directly or via secrets.yaml)

See the esphome [Wireguard component](https://esphome.io/components/wireguard.html) documentation for details and peer setup guide.

If you will have the Magic Clock in the same network as your Home Assistant server, you can remove the wireguard setup by removing the _wg:_ section from the configuration.

## Installation

Behind the scenes, esphome will take your configuration file and compile an individual firmware file for your micro controller platform. This firmware needs to be programmed into your controllers firmware memory. For the first time, this requires a USB connection to your computer. Later updates can be installed Over the Air [(OTA)](https://esphome.io/components/ota/esphome.html)

The Installation guide at [https://esphome.io/guides/getting_started_hassio](https://esphome.io/guides/getting_started_hassio) will guide you through installing the esphome Device builder as a Home Assistant Plugin and create and flash your first esphome firmware. If you can't install Home Assistant pluginst, esphome can be run as a [docker container](https://esphome.io/guides/getting_started_command_line#esphome-device-builder-docker).

Both options will give you access to the esphome Device manager in your webbrowser where you can add your Magic Clock as a device and upload your individual configuration file, create a firmware file based on it and flash that file on the micro controller.

For further help on esphome check out the Guides and FAQs and tips available on the [esphome homepage](https://esphome.io/).

## Digital Twin

The esphome configuration for the digital twin is found in the file [display.yaml](display.yaml). It acts as a remote control for a "big" version. So it connects to the LED entities provided by the controlled device and has a drop down field to set the input_select entity in the backend that drives the clock hand. If you want to use _only_ a digital version you need to edit the firmware configuration to either provide entities for the two LEDs or add helpers in the back end and use those.

Also, it does _not_ include a Wireguard configuration as that device is staying within my wifi. But you can set it up in the same way as it is included in magic-clock.yaml

