
# M-Gauge


**M-Gauge** is a customizable digital gauge display for land based vehicles, designed to present engine and vehicle information clearly, efficiently, and with style. M-Gauge renders configurable gauge screens, receives ECU data over CAN bus, supports user configurable favorites/settings stored internally, includes optional WiFi, Web configuration utility, and ESP-NOW wireless gauge features.
<img width="1054" height="136" alt="M-Gauge_githun_header_2" src="https://github.com/user-attachments/assets/7ec34bf1-8416-477b-9aca-12d3db672f73" />


Built around the **LilyGO T-Display S3 AMOLED 1.75" round display** featuring an **ESP32-S3 microcontroller**.

https://lilygo.cc/products/t-display-s3-amoled-1-64

Other ESP32-S3 based round display modules (Waveshare, etc.) should work with minor modifications.

___
**UPDATE!** Waveshare ESP32-S3 1.75 AMOLED and ESP32-S3 4"LCD square format development boards tested and working with M-Gauge!

Waveshare ESP32-S3 Round 1.75" Brings also additional new and cool features like built-in 3-axis accelorometer, etc.. 
* https://www.waveshare.com/esp32-s3-touch-amoled-1.75.htm

--

Waveshare ESP32-S3 LCD4 is basically plug and play for automotive use, no soldering or internal wiring required.
* Not as good readability in direct sunlight as on the other boards, and generally not as nice display as AMOLED, also slightly slower performance. 
* https://www.waveshare.com/esp32-s3-touch-lcd-4.htm

Updated firmware binary for Waveshare boards available in the firmware download section

**NOTE!** 
1. When ordering Waveshare boards make sure to select version with **touch display** 
2. Waveshare boards DO NOT currently have tested and functional enclosure!! ONLY the LilyGo board has a 3D-printed enclosure currently available!! 
___

**Short teaser video below**

[VIDEO of M-Gauge features](https://flic.kr/p/2snAxms)


___
## Hardware

- ESP32-S3 based round display board
- SN65HVD230 transceiver connected to the configured ESP32 TWAI/CAN pins
- 12V->5V DC-DC Step Down regulator suitable for automotive use  
- Optional SD card for loading of external configurations and custom images
- 3D-model for 3D Printed enclosure available 
---

## Features

### ECU Support
* CAN-Bus communication between ECU and M-Gauge
* Originally designed to work with **MaxxECU**
* Display any available ECU parameter in MaxxECU v1.3 canbus protocol
 * Should just as well work with other ECU's that stream data on CAN-Bus
 * **User configurable CAN Message mapping**
 * Untested configuration files for Megasquirt, Haltec and Holley ECU's available.
* Automatic scaling of engine data to match selected gauge type

---

## Display

### AMOLED Screen

* Excellent readability even in bright sunlight
* Screen brightness control
* Brightness synchronization across multiple M-Gauges
* User selectable delayed boot timing, to stage boot splash screens across multiple M-Gauges

---

## User Interface

### Touch Controls

* Gesture Swipe **Left / Right** to change screens
* Gesture Swipe **Up / Down** to change displayed parameter in current screen
* Long press to open the screen specific parameter selection menu

### Favorites

* Each screen can store up to **10 favorite parameters** for quick access.
<img width="400" height="400" alt="PXL_20260626_120639874 MP" src="https://github.com/user-attachments/assets/2eb71014-8f5e-4595-9fff-0645d4d5b238" />


## Alarm limits
* User configurable minimum/maximum alarm limits
 * Value blinks BLUE on minimum value
 * Value blinks RED on maximum value

## Custom value and gauge decriptor text
* Conditional formatting of screen text
* Custom text can be set on value and gauge decriptor text based on user set conditions
---

## Gauge Layouts

Choose from **6 different screen layouts**:

### (1) Analog Gauge

* Classic analog single gauge display.
<img width="400" height="400" alt="PXL_20260626_120622484 MP" src="https://github.com/user-attachments/assets/6a702391-53d4-445f-9af9-40375557d87a" />


### (2) Dual Analog Gauge

* Display two parameters in classic analog gauge simultaneously.
<img width="400" height="400" alt="PXL_20260626_120612946 MP" src="https://github.com/user-attachments/assets/80c7aad1-20e5-4b02-8d8b-bc8717e41c59" />


### (3) Analog Gauge + Status Display

* Analog Gauge combined with ECU status and warning information.
<img width="400" height="400" alt="PXL_20260626_120604008 MP" src="https://github.com/user-attachments/assets/48a45085-5489-4d58-bd8b-4112be841997" />


### (4) Multi-Data Dashboard

* Display up to **8 parameters** on a single screen with bar type visualization.
<img width="400" height="400" alt="PXL_20260626_120556658" src="https://github.com/user-attachments/assets/665e2cf3-525c-4895-8872-f01eea6247ea" />


### (5) Rolling Data Visualizer

* Real-time graphing of **two user-selected parameters**.
<img width="400" height="400" alt="b615b8e4-89d6-463c-8f92-b31a4742bee2-copied-media~2" src="https://github.com/user-attachments/assets/878bdd44-f71d-427f-896e-4a6106e2214c" />


### (6) Numeric Display simple gauge

* Large numerical value display with two optional visualization bars for maximum readability. Gear, lambda, etc..
* Custom, userdefined colors
<img width="400" height="400" alt="d33cd985-7a68-41cb-8c29-f30157df4d9a-copied-media~2" src="https://github.com/user-attachments/assets/1e963a1f-44cf-4455-848b-afdb904d0ab9" />



### (7) On Screen Gauge Setup
* Easy dropdown configuration menu
<img width="400" height="400" alt="PXL_20260626_120655686" src="https://github.com/user-attachments/assets/47c1c09a-8e5c-4674-85c4-b21e2792e5ce" />


### (8) CAN-Bus / Wireless Button
* Swipe left from Setup Screen to access button
* Pressing button send user defined CAN-Bus message 
---

## Configuration menu

* Easy-to-use gauge configuration menu interface
* Most of the gauge parameters can be changed here
* Changes take effect immediately, but are stored permanently ONLY when swipe up/down is done in any of the gauge data screens
* Some parameters are only avalailable on the text configuration files setup.conf
  * Customized version of these configuration files can be loaded on SD-card. IF SD-card is present and files are found,settings are read during gauge boot sequence and stored in gauge internal memory.  
---

## Shift Light

* Supports **staged shift lights** when using multiple M-Gauge units.
* Each gauge backround turns yellow in sequence (user defined steps)
* At selected shift RPM all gauges turn green
* Define total number of gauges, individual Gauge ID & shift light limit values,in the Setup screen, system will calculate shift light points for each gauge
* Shift light functionality source can be freely chosen from any ECU data, so this can act as some other indicator than shift light just as well...

---

## ECU alarm and Status messages

* ECU alarms shown on screen in rolling red text
* ECU bit status changes in rolling yellow text  , limiters/cuts active, etc...
  
---

# Wait, Theres more....


## Custom CAN-message mapping
Allows M-Gauge to be used with practically any aftermarket ECU or CAN-Bus device that's streams data. 
Well, at least should work...
Factory OBD, PID polling type CAN-Bus devices are not supported.

----
**Note!** This part has been largely replaced by the **Configuration web server** interface. 
SD-Card is not required run M-Gauge!

* On M-Gauge boot, firmware checks SD for file with name /canmap.conf.
* This file contains all information for CAN-Bus message decoding, ID, min/max values, alarm limits and name&units used on screen. 
* If file is found on SD-Card during boot up, it copies it to internal file system LittleFS /canmap.conf.
* on next boot up M-Gauge loads CAN-Bus configuration from internal memory
* SD card  MUST  be removed once configuration update is made, otherwise settings are always reset on boot.
----  


---

# Wireless Features

## OTA Updates
* Update gauge firmware wirelessly over Wifi connectioin without removing the gauge from the dashboard.

### Benefits
* No USB cable required
* No disassembly needed
* Fast firmware deployment to multiple devices

---
## Wireless configuration
 * Configuration web server can be started on the M-Gauge setup page
 * Configuration web page can be accessed by connecting to the gauge wifi accesspoint M-Gauge_AP_0,1,2,3,4
 * Canmap.conf or setup.conf files can be loaded from M-Gauge, edited in the web page and saved back in to M-Gauge 
---

## ESP-NOW Support

M-Gauge supports ESP-NOW for wireless gauge networking.

### How It Works

Only one M-Gauge requires a physical connection to the vehicle CAN-Bus.

One gauge acts as the **ESP-NOW Host**, broadcasting ECU data wirelessly to all other M-Gauges.

Additional gauges receive data wirelessly and do not require CAN wiring.

* minimal to unnoticeable lag in screen update rate

### Tested Range

* Personally tested beyond **100 meters**
* ESP-NOW has been reported to work at distances up to **1 km** under ideal conditions

---

## Wireless Buttons

Touchscreen buttons can transmit user-defined CAN-Bus messages to the ECU.
Examples:
* Launch control enable
* Map switching
* Boost mode selection
* Traction control settings
* Any ECU function controlled by CAN messaging

Works from both:

* Directly connected M-Gauges
* ESP-NOW wireless M-Gauges

### Downside of wireless communication  
 * Currently all ESP-NOW communication in M-Gauge is just broadcasted from the host, unencryped, so basically anyone within range could pick up your engine data
 * Anyone within the range can activate your wireless button
 * Some fixes to this will come in future updates 

---

## Boost level warning system

* "Warning!!!  Danger to manifold"-warning at user selectable boost level ;)

---
# Assembly

### Main Controller

* LilyGO T-Display S3 AMOLED 1.75" ESP32-S3
* Capacitive Touch Interface
  
### CAN-Bus Connectivity

**NOTE!!! The only currently available 3D-printed enclosure model is compatible only with the LilyGo 1.75" with the flat PCB style CAN-Tranceiver!!!!. The plug&play type CAN-transceiver, as well as other boards require full redesign of the enclosure. Anyone interested in different enclosure variants, please open issue here on this project**

  * **This part requires a bit of tinkering...** soldering, or just careful bending of pins depending on what components you choose.. 
  * **SN65HVD230** CAN Bus Transceiver Communication-module is required with Esp32, these can be found online for couple of euros/pcs
  <img width="418" height="169" alt="CAN_Bus Tranceivers" src="https://github.com/user-attachments/assets/6e8bea86-43ca-4eea-bbde-ea8c9b25e45d" />

  * Either of the two versions will work. Only challenge is to bend the tranceiver module PCB pins to match pins on the ESP32 GPIO Header
   * 3.3V, GND, TX and RX lines need to be connected between **SN65HVD230** transiever and ESP32
   * Current pinconfig is set up CAN_RX Pin 42 , CAN_TX pin 43,   
   * CAN H and CAN L from ECU CAN-Bus need to be connected to **SN65HVD230** transiever respective connection points
   * If using flat **SN65HVD230** "flat" PCB , Pins on the PCB can be quite easily bent so that all pins match ESP32 I/O-header. 
     * The Tranceiver PCB lays flat on top of the microcontroller PCB making the assembly very compact. Downside is that some soldering is required.
   * the plug-in version of **SN65HVD230** also requires some bending of pins to match teh GPIO, other options is to get extension header wire. This option takes up       more space. 
   
   * Below image uses CAN_RX Pin 41 , CAN_TX pin 42

     <img width="420" height="389" alt="M-Gauge_GPIO_Pinout" src="https://github.com/user-attachments/assets/c7e179d3-0139-4736-9555-8fc8bd2c237f" />
   
   * Alternatively CAN_RX Pin 42 and CAN_TX pin 43 can be used, this allows clean packaging when using solder in PCB type tranceiver
     <img width="600" height="526" alt="Pinout" src="https://github.com/user-attachments/assets/96845cf6-767b-4e1a-8850-05a2c4fc3297" />

   * Can-Bus Tranceiver PCB soldered in to the GPIO-Pin Header and assembled to the 3D printed enclosure.
     <img width="800" height="800" alt="Assembly_1" src="https://github.com/user-attachments/assets/83512ebb-7542-4fbf-956f-f070f6c03470" />

   * Another option is to use this type of CAN-Bus tranceiver board. Requires basically only a bit of bending of conection pins as shown
   * This would be the plug&play solution...
   <img width="800" height="800" alt="Assembly_2" src="https://github.com/user-attachments/assets/176b7f8a-b75d-43d0-bc18-586de07d6014" />



   

### Power source
* Some type of automotive grade power supply to provide stable 5V to ESP32 is required!
  * DC-DC Step Down Power Supply Module DC 7-100V to DC 5V 3A from Aliexpress has worked for me
  <img width="455" height="226" alt="DC-DC_Regulator" src="https://github.com/user-attachments/assets/f356a263-5a68-4ab7-92a0-42cf15032f67" />
  
  https://www.aliexpress.com/item/1005005978350087.html

* 3D printed casing is designed to house this small regulator board inside
    
  * Only one M-Gauge requires power regulator, the rest of the units can be just daisy chained to the 5V source on the first one.
    That is, just as long the output of the reguator is not exceeded   
  * Basically you could also just use any of those automotive USB cellphone chargers just as well and plug into the ESP32 USB port!
    * Down side to this approach is that USB cable may limit gauge installation locations

  * 3D-printed enclosure/casing
   * Not absolutely required but makes installation of the display easier, keeps wires tidy inside the enclosure etc, etc...
   * Step / STL file available for download

---

# Roadmap

### Planned Features
* Make remote button feature more secure, currently anyone can connect to ESP-Now network and activate button remotely ;)


---

# License

M-Gauge is licensed under the M-Gauge Non-Commercial License.
Commercial use is not allowed.

See the LICENSE file for details.

---
# INSTALLATION
M-Gauge binary package can be uploaded to ESP32-S3 board most simply by using online FLashin service such as: 
https://www.espboards.dev/tools/program/

<img width="1523" height="1024" alt="M-Gauge_Flashing_instructions" src="https://github.com/user-attachments/assets/73e57550-637f-4c54-8c82-08ae425fcbfd" />


Alternatively, use esptools

---
## Configuration

Default persistent settings and favorites are stored in:
```text
setup.conf
```
ECU/display configuration can be loaded from  SD card as:
```text
canmap.conf
```
See attached setup.conf files

 * IF either of above files are present on SD card mounted to the gauge at boot time settings from those files are loaded to gauges internal memory
 * * **REMOVE SD CARD from device if you dont want ALL your settings to be reset all times!!**
  
### Configuration Web Server
All settings found in setup.conf and canmap.conf can be edited directly in from a Web Browser
* Enable Configuration Web Server and Wifi on the Setup page
* Connect your wifi to M-Gauge access point
* open your favorite browser and type in 192.168.0.10(0,1,2,3,4). The last number is the Gauge_ID of your M-Gauge, which you set in the Setup Page 
<img width="400" height="860" alt="Configuration" src="https://github.com/user-attachments/assets/b52c6959-870f-4c1a-beec-86805ede102d" />

# M-Gauge Configuration Guide

M-Gauge uses two configuration files:

- `setup.conf` stores gauge settings, favorites, and the startup screen.
- `canmap.conf` tells M-Gauge how to read and display ECU CAN data and internal inputs.

Both files are stored in LittleFS. A `canmap.conf` found on the SD card is copied to LittleFS during startup. Settings changed on the gauge are saved back to `setup.conf`, so manual changes to that file may later be overwritten.

## General value formats

- `0` means **Off** and `1` means **On** for most switches.
- CAN IDs can be decimal or hexadecimal. Hexadecimal values start with `0x`, for example `0x500`.
- Signal and screen numbering starts at `0`.
- Keep parameter names and letter case exactly as shown.

# setup.conf

Ordinary settings use this format:

```text
SETTING:parameter:value
```

Example:

```text
SETTING:brightness:180
SETTING:wifiEnabled:1
```

## Gauge and display settings

| Parameter | Values | Description |
|---|---:|---|
| `brightness` | `10` to `255` | Screen brightness. A higher number makes the display brighter. |
| `gaugeId` | `0` to `10` | Number that identifies this gauge when several M-Gauges are linked. Each gauge should use a different ID. |
| `gaugeCanId` | `0x300` to `0x500` | Base CAN ID used for communication between M-Gauges. The gauge ID is added to this value. |
| `gaugeCount` | `1` to `16` | Total number of M-Gauges in the linked system. |
| `selectedSetting` | menu row number | Remembers the last selected item on the Gauge Setup screen. Normally this should not be edited manually. |
| `simpleGaugeIndicatorVersion` | `3` | Internal compatibility marker for the Simple Gauge Indicator setting. Keep this at `3`. |
| `simpleGaugeIndicator` | `0` to `4` | Simple gauge indicator: `0` Bar, `1` Arc, `2` Arc Image, `3` None, `4` Both bar and arc. |
| `simpleGaugeColor` | `0` to `255` | Color selection used by the simple gauge bar and all basic arcs. |
| `simpleOverlayImage` | `0` or `1` | Shows the custom `GaugeBG5.png` overlay on the simple gauge when enabled. |
| `limitGaugeValues` | `0` or `1` | Keeps displayed values within the `min` and `max` limits defined in `canmap.conf`. |
| `showNeedle` | `0` to `2` | Main gauge indicator: `0` Arc, `1` Needle and arc, `2` Needle only. |
| `needleType` | `0` or `1` | Needle image: `0` built-in needle, `1` custom `Needle.png`. |
| `arcType` | `0` or `1` | Arc style on gauge screens 1 to 3: `0` custom image, `1` basic colored arc. |
| `chartUpdateRateMs` | `0` to `100` | Delay between chart updates in milliseconds. Lower values are faster but use more processing power. |

## Startup and button settings

| Parameter | Values | Description |
|---|---:|---|
| `bootDelayMs` | `0` to `2000` | Extra delay before startup, in milliseconds. |
| `showBootImage` | `0` or `1` | Shows the boot image during startup when enabled. |
| `buttonMode` | `0` or `1` | Opens the dedicated button-image screen at startup when enabled. |
| `buttonPressedCanEventId` | `0` to `255` | Event number sent over the M-Gauge CAN link when the button screen is pressed. |

## CAN settings

| Parameter | Values | Description |
|---|---:|---|
| `canBusLink` | `0` or `1` | Enables CAN reception and communication between linked M-Gauges. |
| `canRxPin` | `0` to `48` | GPIO pin connected to CAN receive (RX). Use the value required by your board wiring. |
| `canTxPin` | `0` to `48` | GPIO pin connected to CAN transmit (TX). Use the value required by your board wiring. |
| `canRsPin` | `-1` to `48` | GPIO pin used for CAN transceiver standby or mode control. Use `-1` when it is not connected. |

## WiFi, ESP-NOW, and debugging

| Parameter | Values | Description |
|---|---:|---|
| `wifiEnabled` | `0` or `1` | Enables the M-Gauge WiFi access point. |
| `wifiPassword` | 8 to 10 characters | Password for the M-Gauge WiFi access point. |
| `espNowEnabled` | `0` or `1` | Enables wireless ESP-NOW communication between gauges. |
| `espNowHostMode` | `0` to `2` | ESP-NOW role: `0` Client, `1` Host, `2` One-to-many host. |
| `espNowMacFilter` | `0` or `1` | Allows a client to accept data only from its saved host when enabled. |
| `espNowHostMac` | MAC address | Saved host address, for example `AA:BB:CC:DD:EE:FF`. This is normally managed automatically. |
| `espNowPeerMacs` | MAC address list | Comma-separated list of ESP-NOW peers. This is normally managed automatically. |
| `telnetEnabled` | `0` or `1` | Enables remote debug messages over the network. |

The **Web Config Server** switch is controlled from the Gauge Setup screen. It is not currently stored as a separate `setup.conf` parameter.

## Shift light, alarms, and status

| Parameter | Values | Description |
|---|---:|---|
| `revLimitSourceIndex` | `0` to `83` | Signal number from `canmap.conf` used to control the shift light. This is normally the RPM signal. |
| `shiftLight` | signal value | Value at which the shift light becomes active. Its unit follows the selected shift-light signal. |
| `shiftLightStep` | signal value | Difference between the shift-light points of consecutive linked gauges. Use `0` for the same point on every gauge. |
| `demoBoostLimitX10` | value x 10 | Boost warning limit in tenths of a bar. For example, `15` means `1.5 bar`. |
| `displayErrors` | `0` or `1` | Shows ECU error messages when enabled. |
| `displayStatus` | `0` or `1` | Shows active status-bit messages when enabled. |
| `alarmMonitorEnabled` | `0` or `1` | Monitors the signals listed in `ROLE:alarmMonitor` and warns when a limit is exceeded. |

## Internal sensor settings

| Parameter | Values | Description |
|---|---:|---|
| `accelerometerEnabled` | `0` or `1` | Enables the board motion sensor for accelerometer and gyroscope values. |
| `internalInputsEnabled` | `0` or `1` | Enables user-defined GPIO inputs from `INTERNAL:` rows in `canmap.conf`. |

## Screen, selection, and favorite rows

These rows do not begin with `SETTING:` DO NOT MODIFY THESE MANUALLY!.

| Row | Description |
|---|---|
| `SELECTED:<screen>: <signals>` | Signal numbers currently shown in the gauge positions on that screen. |
| `FAVORITES:<screen>: <signals>` | Signal numbers available in that screen's Favorites list. A screen can have up to 10 favorites. |
| `STARTSCREEN:<screen>` | Screen opened after startup. Valid screen numbers are `0` to `5`. |

Example:

```text
SELECTED:0: 1,2,3
FAVORITES:0: 1,2,3,18
STARTSCREEN:0
```

Screen `0` is the first gauge screen and screen `5` is the sixth gauge screen. Signal numbers refer to `SIGNAL:` or `INTERNAL:` numbers in `canmap.conf`.

# canmap.conf

`canmap.conf` defines where gauge data comes from and how it is shown. Blank lines and lines beginning with `#` or `;` are ignored, so they can be used for comments.

The recommended format keeps one complete definition on one line:

```text
SIGNAL:0,id=0x520,startBit=0,bitLength=16,endian=little,signed=0,factor=1,offset=0,scaling=1,min=0,max=8000,alarmLimitMin=0,alarmLimit=7000,alarmSource=0,name=RPM|Engine Speed
```

## CAN bus speed

```text
SPEED:500KBITS
```

| Parameter | Description |
|---|---|
| `SPEED` | CAN bus speed. Supported values are `1`, `5`, `10`, `12.5`, `16`, `20`, `25`, `50`, `100`, `125`, `250`, `500`, `800` kbit/s and `1` Mbit/s. `500KBITS` is the normal default. |

Examples such as `500K`, `500KBITS`, `500000BITS`, and `1MBITS` are accepted.

## CAN receive filter

```text
FILTER:id=0x500,mask=0x700,extended=0
```

| Field | Values | Description |
|---|---:|---|
| `id` | CAN ID | Base ID that the filter compares against incoming CAN messages. `canId` is also accepted. |
| `mask` | bit mask | Selects which parts of the CAN ID must match. A wider filter reduces unwanted CAN traffic. |
| `extended` | `0` or `1` | Use `0` for normal 11-bit CAN IDs and `1` for extended 29-bit IDs. `extd` is also accepted. |

If the filter is missing or invalid, M-Gauge accepts all CAN IDs.

## Signal roles

Roles tell M-Gauge which configured signals have special jobs.

```text
ROLE:rpmIndex=0
ROLE:mapIndex=1
ROLE:errorCountIndex=50
ROLE:errorCodeIndex=51
ROLE:alarmMonitor=3,5,18
```

| Role | Description |
|---|---|
| `rpmIndex` | Signal number used as engine RPM. |
| `mapIndex` | Signal number used as manifold pressure or boost. |
| `errorCountIndex` | Signal number that reports how many ECU errors are active. |
| `errorCodeIndex` | Signal number that reports the current ECU error code. |
| `alarmMonitor` | Comma-separated signal numbers to watch for low or high alarms. Up to 10 signals can be listed. |

## CAN signals

A `SIGNAL:` row creates one value that can be selected and displayed on the gauge. Signal numbers are `0` to `83`.

```text
SIGNAL:3,id=0x520,startBit=48,bitLength=16,endian=little,signed=1,factor=1,offset=0,scaling=10,min=-40,max=150,alarmLimitMin=-20,alarmLimit=110,alarmSource=0,name=C|Coolant Temperature
```

| Field | Values | Description |
|---|---:|---|
| `id` | CAN ID | CAN message that contains this value. `canId` is also accepted. |
| `startBit` | `0` to `63` | Position of the first data bit inside the CAN message. |
| `bitLength` | `1` to `64` | Number of bits used by the value. Common sizes are `8`, `16`, and `32`. |
| `endian` | `little` or `big` | Byte order used by the ECU for this value. Use the value stated in the ECU CAN documentation. |
| `signed` | `0` or `1` | Use `1` if the value can be negative; otherwise use `0`. |
| `valueType` | `integer` or `float32` | Data format. Most signals use `integer`. A 32-bit IEEE floating-point signal uses `float32`. `type` and `format` are accepted as alternate field names. |
| `factor` | number | Multiplies the received value to convert it to the required scale. Usually `1`. |
| `offset` | number | Amount added after the factor. Usually `0`. |
| `scaling` | positive whole number | Divides the stored value for display. For example, stored value `123` with scaling `10` is shown as `12.3`. |
| `min` | number | Lowest value used by the gauge scale and optional value limiting. |
| `max` | number | Highest value used by the gauge scale and optional value limiting. |
| `alarmLimitMin` | number | Low alarm point for this signal. |
| `alarmLimit` | number | High alarm point for this signal. |
| `alarmSource` | number | Reserved compatibility field. It is stored but currently does not change alarm behavior. Use `0`. |
| `name` | `unit|description` | Unit and user-visible name. For example, `C|Coolant Temperature`. Use `|Gear` when no unit is needed. |

The basic conversion is:

```text
displayed value = ((received value x factor) + offset) / scaling
```

Use the factor, offset, byte order, and bit position specified in the ECU manufacturer's CAN documentation.

## Status bits

A `BIT:` row reads one on/off condition from a CAN message. Bit numbers are `0` to `15`.

```text
BIT:0,id=0x530,startBit=2,name=Launch Control Active
```

| Field | Values | Description |
|---|---:|---|
| `id` | CAN ID | CAN message that contains the status bit. `canId` is also accepted. |
| `startBit` | `0` to `63` | Position of the status bit inside the message. |
| `name` | text | Message shown when this status is active. Use `|` where a line break is wanted. |

Status messages are shown only when `SETTING:displayStatus:1` is enabled.

## Text shown for specific values

`VALUE:` replaces a number with text. `UNIT:` changes the unit or secondary text for a matching value. Each rule refers to a signal number.

```text
VALUE:57,raw=0,text=N
VALUE:57,raw=1,text=1
UNIT:57,raw=0,text=Neutral
UNIT:57,raw>10,text=Gear Failure
```

| Field | Description |
|---|---|
| `raw=number` | Applies the rule when the stored signal value equals the number. |
| `raw>number` | Applies when the stored value is greater than the number. |
| `raw<number` | Applies when the stored value is less than the number. |
| `raw>=number` | Applies when the stored value is equal to or greater than the number. |
| `raw<=number` | Applies when the stored value is equal to or less than the number. |
| `text` | Text to show when the rule matches. Use `|` where a line break is wanted. |

Rules compare the stored value after `factor` and `offset`, but before `scaling`. M-Gauge supports up to 32 `VALUE:` rules and 32 `UNIT:` rules.

## ECU error descriptions

An `ERROR:` row gives a readable message to an ECU error code.

```text
ERROR:12,text=Coolant temperature sensor fault
```

| Field | Description |
|---|---|
| Number after `ERROR:` | ECU error code from `0` to `254`. |
| `text` | Error message shown to the user. Use `|` where a line break is wanted. |

Error messages are shown only when `SETTING:displayErrors:1` is enabled and the error role signals are configured.

## Internal motion sensors

An `INTERNAL:` row can place a board accelerometer or gyroscope value in any signal slot. The number after `INTERNAL:` is the signal number shown in Favorites.

```text
INTERNAL:25,source=accelX,factor=1,offset=0,scaling=1000,min=-2,max=2,alarmLimitMin=-2,alarmLimit=2,alarmSource=0,name=g|Acceleration X
```

| Field | Values | Description |
|---|---:|---|
| `source` | `accelX`, `accelY`, `accelZ`, `gyroX`, `gyroY`, `gyroZ` | Motion sensor axis used as the value source. |
| `factor` | number | Multiplies the sensor value. Usually `1`. |
| `offset` | number | Amount added after the factor. Usually `0`. |

The display fields `name`, `scaling`, `min`, `max`, `alarmLimitMin`, `alarmLimit`, and `alarmSource` work the same way as on a `SIGNAL:` row. Enable `SETTING:accelerometerEnabled:1` to update these values.

## Internal GPIO inputs

An `INTERNAL:` row with `source=gpio` displays a digital or analog GPIO input as a gauge signal.

```text
INTERNAL:28,source=gpio,pin=21,mode=analog_mv,invert=0,factor=1,offset=0,scaling=1000,min=0,max=3.3,alarmLimitMin=0,alarmLimit=3.3,alarmSource=0,name=V|Sensor Input
```

| Field | Values | Description |
|---|---:|---|
| `source` | `gpio` | Selects a user GPIO pin as the value source. |
| `pin` | allowed GPIO number | Physical input pin to read. |
| `mode` | see list below | Selects digital or analog input behavior. |
| `invert` | `0` or `1` | Reverses digital input values when set to `1`. It has no effect on analog inputs. |
| `factor` | number | Multiplies the pin reading. Usually `1`. |
| `offset` | number | Amount added after the factor. Usually `0`. |

Available GPIO modes:

| Mode | Description |
|---|---|
| `digital` | Normal digital input. Displays `0` or `1`. |
| `digital_pullup` | Digital input with the ESP32 internal pull-up resistor enabled. |
| `digital_pulldown` | Digital input with the ESP32 internal pull-down resistor enabled. |
| `analog_raw` | Displays the raw analog-to-digital converter reading. |
| `analog_mv` | Displays the measured input in millivolts before display scaling. |

The display fields `name`, `scaling`, `min`, `max`, `alarmLimitMin`, `alarmLimit`, and `alarmSource` work the same way as on a `SIGNAL:` row. Enable `SETTING:internalInputsEnabled:1` to update GPIO values.

Current LilyGo GPIO allowlist: `1`, `2`, `3`, `5`, `21`, `38`, `39`, `45`, `46`, `47`. Up to six GPIO inputs can be configured. GPIO inputs are currently supported on the LilyGo build; Waveshare builds ignore them.

> Check the board wiring before selecting a pin. The user is responsible for choosing a suitable digital or analog pin and keeping the input voltage within the ESP32 electrical limits.

An `INTERNAL:` definition replaces CAN decoding for the same signal number. Define `source` before the other input fields on that row.

## Minimal canmap.conf example

```text
# CAN bus setup
SPEED:500KBITS
FILTER:id=0x500,mask=0x700,extended=0

# Special signal roles
ROLE:rpmIndex=0
ROLE:mapIndex=1
ROLE:errorCountIndex=50
ROLE:errorCodeIndex=51
ROLE:alarmMonitor=3,5

# Gauge signals
SIGNAL:0,id=0x520,startBit=0,bitLength=16,endian=little,signed=0,factor=1,offset=0,scaling=1,min=0,max=8000,alarmLimitMin=0,alarmLimit=7000,alarmSource=0,name=RPM|Engine Speed
SIGNAL:1,id=0x520,startBit=16,bitLength=16,endian=little,signed=0,factor=1,offset=0,scaling=10,min=0,max=300,alarmLimitMin=0,alarmLimit=250,alarmSource=0,name=kPa|Manifold Pressure

# Status and text examples
BIT:0,id=0x530,startBit=2,name=Launch Control Active
VALUE:57,raw=0,text=N
UNIT:57,raw=0,text=Neutral
ERROR:12,text=Coolant temperature sensor fault
```

Restart M-Gauge after replacing `canmap.conf` so the new mapping is loaded.


