# WirelessPrinting [![Build Status](https://travis-ci.org/probonopd/WirelessPrinting.svg?branch=master)](https://travis-ci.org/probonopd/WirelessPrinting)

![icon](https://cloud.githubusercontent.com/assets/2480569/23587222/bb25f740-01a7-11e7-806f-23c8f77d8b1c.png)

Allows you to print from [Cura](https://ultimaker.com/en/products/cura-software) to your 3D printer connected to an [ESP8266](https://espressif.com/en/products/hardware/esp8266ex/overview) module.

__UNDER DEVELOPMENT__. See [Issues](https://github.com/probonopd/WirelessPrinting/issues). Pull requests welcome!

## ESP8266WirelessPrint

The [esp8266/Arduino](https://github.com/esp8266/Arduino) sketch `ESP8266WirelessPrint.ino` is uploaded to a ESP8266 module. 

### Compiling

As or March 2017, this code compiled on Arduino hourly and esp8266/Arduino git master. See `.travis.yml` for how this is compiled on Travis CI.

### Connection

The ESP8266 module is connected with your 3D printer via the serial connection and to a SD card (acting as a cache during printing).

You need to connect
* TX, RX from your 3D printer to the ESP8266 module (__EXT-1__ header on RAMPS boards)
* Power and GND from your 3D printer to the ESP8266 module (attention, the __EXT-1__ header on RAMPS boards has 5V while the ESP8266 needs 3.3V)
* SD card shield to the ESP8266 module (a capacitor across the power pins of the SD card; SD shields have this)

### A note on SD cards

Using a SanDisk 2 GB card formatted with the [SD Card Formatter](https://www.sdcard.org/downloads/formatter_4/) from the SD Association seems to work for me

### Configuration

On the SD card, create a file called `config.ini` with the following content:

```
[network]
ssid=<the name of your wireless network>
password=<the password of your wireless network>
hostname=<the desired hostname for the machine, e.g., "3d">
```

### Usage

To print, just open http://the-ip-address/ and upload a G-Code file using the form:

![Upload](https://cloud.githubusercontent.com/assets/2480569/23586936/fd0e3fa2-01a0-11e7-9d83-dc4e7d031f30.png)

Ycan also print from the command line using curl:

```
curl -F "file=@/path/to/some.gcode" http://the-ip-address/print
```

## WirelessPrinting for Cura

Cura plugin which discovers ESP8266WirelessPrint instances using Zeroconf and enables printing directly to ESP8266WirelessPrint.

### Installation

- Make sure your Cura version is 2.4 or newer. On Linux, download [Cura-2.5.0.AppImage](http://software.ultimaker.com/current/Cura-2.5.0.AppImage).
- Download or clone the repository into [Cura installation folder]/plugins/WirelessPrinting 
  or in `~/.local/share/cura/plugins` on Linux. The folder of the plugin itself *must* be ```WirelessPrinting```

```
mkdir -p ~/.local/share/cura/plugins
( cd ~/.local/share/cura/plugins ; git clone https://github.com/probonopd/WirelessPrinting.git )
```

### How to use

- Make sure WirelessPrint is up and running, and the discovery plugin is not disabled
- In Cura, add a Printer matching the 3D printer you have connected to WirelessPrint
- Select "Connect to WirelessPrint" on the Manage Printers page.
- Select your WirelessPrint instance from the list
- From this point on, the print monitor should be functional and you should be
  able to switch to "Print on <devicename>" on the bottom of the sidebar.
- A matching case for a WeMos D1 mini and microSD shield can be found at http://www.thingiverse.com/thing:2287618
