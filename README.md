
# M-Gauge


**M-Gauge** is a customizable digital gauge display for land based vehicles, designed to present engine and vehicle information clearly, efficiently, and with style. M-Gauge renders configurable gauge screens, receives ECU data over CAN bus, supports user configurable favorites/settings stored internally, includes optional WiFi, Over the air updates and ESP-NOW features.
<img width="1054" height="136" alt="M-Gauge_githun_header_2" src="https://github.com/user-attachments/assets/7ec34bf1-8416-477b-9aca-12d3db672f73" />


Built around the **LilyGO T-Display S3 AMOLED 1.75" round display** featuring an **ESP32-S3 microcontroller**.

https://lilygo.cc/products/t-display-s3-amoled-1-64

Other ESP32-S3 based round display modules (Waveshare, etc.) should work with minor modifications.

___
**UPDATE!** Waveshare 1.75 AMOLED tested and working with minor software modifications.

Brings also additional new and cool features like built-in 3-axis accelorometer, etc..

https://www.waveshare.com/esp32-s3-touch-amoled-1.75.htm

Updated firmware binary for Waveshare board available here soon..
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
* Currently supports **MaxxECU standalone ECUs**
* Display any available ECU parameter in MaxxECU v1.3 canbus protocol
 * Should just as well work with other ECU's that stream data on CAN-Bus
* **User configurable CAN Message mapping**
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
* Define total number of gauges, individual Gauge ID & shift light limit values,in the Setup screen, system will calculate shift light points for each gauge
* Shift light functionality source can be freely chosen from any ECU data, so this can act as some other indicator than shift light just as well...

---

## ECU alarm and Status messages

* ECU alarms shown on screen in rolling red text
* ECU bit status changes in rolling yellow text  , limiters/cuts active, etc...
  
---

# Wait, Theres more....


## Custom CAN-message mapping
Allows M-Gauge to be used with practically any ECU or CAN-Bus device. 
Well, at least should work... Has not been tested with any other ECU than MaxxECU Race.
  
* On M-Gauge boot, firmware checks SD for file with name /canmap.conf.
* This file contains all information for CAN-Bus message decoding, ID, min/max values, alarm limits and name&units used on screen. 
* If file is found on SD-Card during boot up, it copies it to internal file system LittleFS /canmap.conf.
* on next boot up M-Gauge loads CAN-Bus configuration from internal memory
* SD card  MUST  be removed once configuration update is made, otherwise settings are always reset on boot.
   
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

