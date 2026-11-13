
# M-Gauge

**M-Gauge** is a customizable digital gauge display for land based vehicles, designed to present engine and vehicle information clearly, efficiently, and with style. M-Gauge renders configurable gauge screens, receives ECU data over CAN bus, supports user configurable favorites/settings and custom background images  stored internally, includes optional WiFi, Web configuration utility, and ESP-NOW wireless gauge feature.
<img width="1054" height="136" alt="M-Gauge_githun_header_2" src="https://github.com/user-attachments/assets/7ec34bf1-8416-477b-9aca-12d3db672f73" />

3 development board displays are supported:

The original **LilyGO T-Display S3 AMOLED 1.75" round display** featuring an **ESP32-S3 microcontroller**
* https://lilygo.cc/products/t-display-s3-amoled-1-64
  

Waveshare ESP32-S3 Round 1.75" Brings also additional cool features like built-in 3-axis accelorometer, etc.. 
* https://www.waveshare.com/esp32-s3-touch-amoled-1.75.htm
  

Waveshare ESP32-S3 LCD4 is basically plug and play for automotive use, no soldering or internal wiring required.
* Not as good readability in direct sunlight as on the other boards, and generally not as nice display as AMOLED, also slightly slower performance. 
* https://www.waveshare.com/esp32-s3-touch-lcd-4.htm

Updated firmware binary for Waveshare boards available in the firmware download section

**NOTE!** 
1. When ordering Waveshare boards make sure to select version with **touch display** 
2. Waveshare boards DO NOT currently have tested and functional enclosure!! ONLY the LilyGo board has a 3D-printed enclosure currently available!! 
___


# M-Gauge 2.0 is here! — More than just a gauge
M-Gauge 2.0 brings together new and exciting features: deeper visual customization, more onboard sensor inputs, a redesigned browser interface, display-free gateway operation, and more capable wireless ESP-NOW gauge networks.

https://github.com/Mr-Mika/M-Gauge/tree/main/Firmware/M-Gauge%202.0

Whether you want one personalized instrument or several linked displays sharing a single CAN connection, this release gives you more control over how your M-Gauge system looks and works.

<img width="300" height="300" alt="M-Gauge_2_screens1" src="https://github.com/user-attachments/assets/28b9b08f-288b-4ee6-833d-9ca63ae1c896" /><img width="300" height="300" alt="M-Gauge_2_screens2" src="https://github.com/user-attachments/assets/6ec3baa1-3cce-474c-948c-dfe3533fdbb7" /><img width="300" height="300" alt="M-Gauge_2_screens3" src="https://github.com/user-attachments/assets/fee4f08c-9411-4bb4-a5e4-436c1f88090f" />

## Native ESP-IDF and expanded hardware support

The firmware now runs natively on ESP-IDF, with dedicated display and touch support for:

- LilyGo 1.75-inch round AMOLED.
- Waveshare 1.75-inch round AMOLED.
- Waveshare 4-inch LCD, using the supported LCD4 board profile.

Hardware-specific features depend on the fitted sensors and selected board profile. In particular, microphone input and the drag performance meter are currently available on the Waveshare round build, not every supported board.

## Make the display your own

- **Independent colors:** customize main numeric values, normal dial ticks, alarm-limit ticks, simple-gauge bars and arcs, and vector needles. Use the setup color sliders or edit the individual RGB hex colors in `setup.conf`.
- **Custom needle styles:** choose a built-in or custom image, a generated **Vector image**, or a live-rendered **Vector needle**. Configure vector needle base width, tip width and length, plus individual needle pivot positions and radial offsets.
- **Adjustable layouts:** reposition the main values and description labels on needle screens, including independent upper and lower labels on the dual-needle screen.
- **More display choices:** select four- or eight-value dashboard layouts, adjust bar corner radius, use a horizontal simple-gauge bar, toggle its shadow, or hide numeric values on the needle screens.
- **Custom artwork and animation:** use custom backgrounds, arcs, needle and button images. PNG images remain supported, with animated GIF support added for boot and dedicated button images. Boot animation plays once; button GIF animation runs while the button is pressed.
- **Peak markers:** choose **Off**, **Lagging Max** for a held peak that fades away, or **Max Value** to retain the peak without fading. Multiple needles and bars track their peaks independently.
- **Secondary-color locking:** tap the setup value to freeze a secondary color while adjusting the primary color. Keep complementary colors, choose matching colors, or create your own combination.

Some advanced layout and geometry options are edited through `setup.conf`; everyday settings remain accessible from the gauge and Live setup.

## A new web configuration experience

The browser interface has a black-and-gold/bronze design, responsive controls, and firmware-version information in the page headers.

<img width="400" height="600" alt="M-Gauge_web_configuration1" src="https://github.com/user-attachments/assets/c48aa7cb-5b49-43b8-86d2-16e37299bc37" />

The main configuration page lets you:

- Load, edit and save `canmap.conf` and `setup.conf`.
- Upload custom images, with format, dimension and file-size checks before activation.
- View and download supported drag-performance SD logs.
- Install application-only OTA firmware updates while preserving configuration and LittleFS files.

### Live setup — adjust the gauge from your browser

A separate **Live setup** page mirrors the supported setup dropdown and slider controls. Change settings in real time, preview color selections, use temporary value locks, and save your changes without editing configuration text.

<img width="400" height="366" alt="M-Gauge_web_configuration2" src="https://github.com/user-attachments/assets/62ad6166-66ee-4282-ad27-7e02857e87c4" />

The ESP-NOW section shows the configured mode, saved host, registered clients and pairing status. Enable or disable ESP-NOW, change its role, start pairing, and start or stop demo mode remotely.

Live setup provides live configuration controls and device status; it is not a streamed mirror of the gauge display or a live telemetry chart.

## Expanded ESP-NOW networking

**One-to-many mode** lets one CAN-connected host supply several wireless M-Gauge clients, reducing the CAN wiring needed behind the dashboard.

<img width="600" height="450" alt="ESP-NOW_simplified" src="https://github.com/user-attachments/assets/04494ec6-217e-486e-bdbf-5494ba67b41d" />

- Explicit pairing and saved host/client registrations simplify building a network that reconnects after reboot.
- Client host-locking and optional host-MAC filtering help prevent accepting data from an unintended gauge.
- Active-client tracking avoids treating every saved peer as currently connected.
- Optional **M-Gauge frame bundling** carries several CAN frames in one ESP-NOW packet for more efficient transport.
- Configurable peer rate, duplicate-frame interval and optional queue/latency diagnostics support installation-specific tuning.
- Demo generation and forwarding run independently of the host graphics loop, so demanding host screens do not directly control the demo transmission schedule.

**Security note:** pairing, peer registration and MAC filtering provide basic separation between gauge networks, not cryptographic security. The current ESP-NOW transport is unencrypted; do not treat it as confidential or protected against a deliberately spoofed sender.

## Updated CAN buttons and visual status indicators

Alongside the dedicated CAN-button screen, M-Gauge now supports **four additional customizable image buttons**, each with its own CAN event ID, across the supported builds.

<img width="600" height="450" alt="M-Gauge_buttons_simplified" src="https://github.com/user-attachments/assets/548addf3-70a4-4c35-99f5-f915ae612aa2" />

Use them as buttons, status indicators, or both:

- Send a configured CAN event when pressed, including from an ESP-NOW client through its host.
- Link each image to a `BIT:` status using visibility, flashing or opacity behavior.
- Show actionable buttons dimmed when inactive, with a brighter pressed/active state.
- Select status-screen, corner-button or supported gauge-overlay placement. Needle-screen overlays follow an upper arc inside the dial ticks; setup remains free of these controls.

Updated CAN control handling also supports brightness, screen-selection and displayed-signal commands addressed to the gauge itself, while avoiding brightness retransmission loops.

## Accelerometer inputs and drag performance

On supported motion-sensor hardware, accelerometer and gyroscope axes can be configured as `INTERNAL:` signals and selected for display like ECU parameters. Scaling, offsets, zero calibration and the usual signal limits allow these inputs to be adapted to the installation. Enabled internal inputs can also be selected as the shift-light source.

The **Waveshare round drag performance meter** adds:

- **1/8-mile and 1/4-mile modes**, armed by a configurable CAN signal or status-bit trigger and started by detected launch acceleration.
- **60 ft, 330 ft and 990 ft split times**, finish elapsed times and estimated finish speeds, plus elapsed time and distance from launch.
- Results available as selectable gauge values, with launch, split and final-results popups.
- An adjustable acceleration calibration factor.
- Optional **100 ms SD logging**, including acceleration, performance values and a results summary. Up to 100 rotating log files can be opened or downloaded through web configuration.

These are accelerometer-derived estimates, not a replacement for track timing. Mounting, vehicle pitch, vibration and sensor bias affect the results; compare and calibrate against reference measurements.

## Microphone intensity and frequency inputs

The Waveshare round board's dual microphones can now supply **sound intensity** and **dominant frequency** as selectable `INTERNAL:` signals.

Choose Full Range, Bass, Midrange, Treble or a Custom frequency band. Custom low- and high-cut settings let you focus on a particular part of the sound spectrum, and the selected filtering also affects the intensity measurement.

Microphone capture is demand-driven, and frequency analysis runs only when its output is needed. This avoids spending processing resources on unused audio inputs. Sound intensity is a relative level, not a calibrated sound-pressure reading in dB SPL.

## More flexible signals, warnings and testing

- Optional **`smoothingMs`** on CAN and internal signals steadies noisy values such as fuel level. Missing or zero leaves smoothing disabled, and raw telemetry remains unchanged.
- Shared value formatting improves consistent scaling, signed values and decimal precision across screens.
- Conditional **`VALUE:` and `UNIT:`** text overrides, linked unit conditions and configurable **`ERROR:`** descriptions make status messages more adaptable.
- Expanded alarm/status displays, selectable shift-light sources and a separately adjustable **Shift Light Overlimit** threshold give more control over warnings.
- Supported internal inputs include GPIO digital/analog measurements and external **INA226 bus-voltage monitoring**, with user-defined scaling.
- A driving-like demo replaces repetitive min/max sweeps with smooth acceleration, deceleration, coasting and partial-range reversals. Demo data can also be sent to wireless clients, including from a headless node.

## Headless mode — a dedicated CAN-to-wireless gateway

M-Gauge can now operate without a display as a **headless node**. One unit connects to the vehicle CAN bus and forwards data to the gauges in your ESP-NOW network.

Headless mode skips display and touch initialization, LVGL graphics, decoded images and the large graphics buffers, freeing RAM for gateway operation. Its Wi-Fi configuration access and web server remain available, so you can manage the node through the main configuration page and Live setup.

Normal CAN processing and ESP-NOW forwarding continue while browsing in headless mode. Remote pairing, configuration and demo-data generation make it possible to set up and test a display-free installation. Firmware updates pause telemetry, and configuration or radio changes can briefly interrupt communication.

Enable `SETTING:headlessMode:1` in `setup.conf` and reboot. Set it back to `0` and reboot to restore display operation.
- This mode allow user to run M-Gauge 2.0 in basically any EPS32-S3-16-8 Board, and use it as a wireless gateway to supported M-Gauge displays.


**M-Gauge 2.0 combines a customizable instrument, a sensor display, a wireless CAN gateway and a browser-managed gauge network in one firmware platform.**


___
## Hardware

- ESP32-S3 based display board
- SN65HVD230 transceiver connected to the configured ESP32 TWAI/CAN pins
- 12V->5V DC-DC Step Down regulator suitable for automotive use  
- Optional SD card for loading of external configurations and custom images(SD CARD NO LONGER REQUIRED! All configurations changes can be made via WebConfiguration)
- 3D-model for 3D Printed enclosure available
<img width="469" height="300" alt="M-Gauge_wiring_simplified" src="https://github.com/user-attachments/assets/0b8b9b76-558a-432f-93eb-d238d0c50ac9" />
<img width="469" height="300" alt="M-Gauge_wiring_LCD4_simplified" src="https://github.com/user-attachments/assets/53b090a7-70b2-4fa4-8098-e15aaf50a7dd" />


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
SD-Card is **NOT** required run M-Gauge!

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
   * Current pinconfig is set up CAN_RX Pin 41 , CAN_TX pin 42, this is functional ONLy for the LilyGo 1.75" display
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

* **NOTE!! Waveshare LCD4 display only requires change of settings in setup.conf to CAN_RX Pin 0 , CAN_TX pin 6. For Waveshare LCD4, CAN-Bus Connectivity and 5V Power supply installation is not required as the board contains all these items from the factory**  

### Plug & Play gauge cluster using Waveshare LCD4 as wireless hub
This multi M-Gauge setup requires no additional hardware, only simple wiring and careful connections to correct input pins on the boards. Only LCD4 is connect to vehicle CAN-Bus other gauges are wireless! 

<img width="800" height="512" alt="M-Gauge_PnP_wiring" src="https://github.com/user-attachments/assets/bef2c7db-6c12-48ed-b581-a2b6f9e08e50" />

Connecting 2 round devices to LCD4 +5V VCC is safe, possibly up to 3 devices is still safe, more than that can be overloading the LCD4's voltage regulation circuit.
* Similar wiring setup can be used with other ESP32 devices that have built in voltage regulation and CAN-Bus connectivity(must be setup as M-Gauge headless mode)
* LilyGo T-2CAN or similar device
* https://lilygo.cc/products/t-2can?srsltid=AYCX9vKbwLe_V-mCvjKKe_4m5SCWXyjG0KkLGAF50CY2CmWf41H7IyYA 

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

**NOTE! Upload the firmware before making any other physical connections to the display board and verify display functionality using the "Demo - mode" from setup page**
---
## Configuration
# M-Gauge Configuration Guide

M-Gauge uses two configuration files:

- `setup.conf` stores gauge settings, favorites, and the startup screen.
- `canmap.conf` tells M-Gauge how to read and display ECU CAN data and internal inputs.

Both files are stored in M-Gauge internal memory. 

## Web configurator 
M-gauge configuration files are most easily edited from the built-in web configuration tool which you can access with your browser after enabling the **Web Config Server** from M-Gauge setup page. **Web Config Server** will activate a wireless access point the user can connect with their wireless device, computer or mobile device.

Use the existing Wi-Fi password and connect to: `M-Gauge-AP_<gaugeId>` at `http://192.168.0.<100 + gaugeId>`

<img width="800" height="1200" alt="M-Gauge_web_configuration1_larger" src="https://github.com/user-attachments/assets/fce36f23-5311-48ba-9363-f719f6e8a1a9" />

Once configuration files have been changed and file/files saved, M-Gauge needs to be restarted , this can be done by switching off **Web Config Server** which will automatically reboot the system, or by manually, by switching OFF power.
**Web Config Server** is not intended to be running contiously in the backround, except when M-Gauge is set running in **Headless mode**.

** in **Headless mode**, **Web Config Server** is running continuously, to provide user interface for ESP32 units used as CAN->ESP-Now gateway devices that do not have a display. Read more about M-Gauge headless mode at the end of this document. 



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
| `Screen0MainValuePosition` | `x,y` (each -1024…1024) | Centre of `ui_LabelValue`; default `233,233`. |
| `Screen0TextLabelPosition` | `x,y` (each -1024…1024) | Centre of `ui_Label3`; default `236,321`. |
| `Screen1UpperMainValuePosition` | `x,y` (each -1024…1024) | Centre of `ui_DualGaugeValueUpper`; default `233,208`. |
| `Screen1UpperTextLabelPosition` | `x,y` (each -1024…1024) | Centre of `ui_DualGaugeUpperLabel`; default `233,153`. |
| `Screen1LowerMainValuePosition` | `x,y` (each -1024…1024) | Centre of `ui_DualGaugeValueLower`; default `233,258`. |
| `Screen1LowerTextLabelPosition` | `x,y` (each -1024…1024) | Centre of `ui_DualGaugeLowerLabel`; default `233,313`. |
| `Screen2MainValuePosition` | `x,y` (each -1024…1024) | Centre of `ui_GaugeValueUpper`; default `231,208`. |
| `Screen2TextLabelPosition` | `x,y` (each -1024…1024) | Centre of `ui_GaugeUpperLabel`; default `231,153`. |
| `simpleGaugeIndicator` | `0` to `5` | Simple gauge indicator: `0` Bar, `1` Arc, `2` Arc Image, `3` None, `4` Both bar and arc, `5` Horizontal Bar. |
| `simpleGaugeColor` | `0` to `255` | Simple-gauge slider position and legacy color fallback; explicit RGB settings below take precedence. |
| `simpleGaugeBarColor` | `0x000000` to `0xFFFFFF` | Actual bar RGB color, including its shadow. |
| `simpleGaugeArcColor` | `0x000000` to `0xFFFFFF` | Actual basic-arc RGB color, including the simple gauge and needle-screen basic arcs. |
| `mainValueColor` | `0x000000` to `0xFFFFFF` | Actual main numeric-value RGB color. |
| `tickValueColor` | `0x000000` to `0xFFFFFF` | Main values color's secondary RGB: simple-gauge arc value and units caption. Does not affect dial ticks. |
| `tickValuesColor` | `0` to `255` | Tick Value Color slider: 0 = white normal ticks / red alarm ticks; 1–10 = gray shades; 11–255 = hue with complementary alarm color. Applies to screens 0–2. |
| `dialTickColor` | `0x000000` to `0xFFFFFF` | Explicit normal dial-tick RGB, independent of Main values color. |
| `dialAlarmTickColor` | `0x000000` to `0xFFFFFF` | Explicit RGB for ticks at or above alarmLimit; default `0xA91A1A`. Tap the Tick Value Color setup value to freeze this secondary color, shown as `(#RRGGBB)`, while adjusting normal ticks. Tap again to restore complementary coloring (white uses default red). The lock clears on leaving the option and is not saved. |
| `vectorNeedleColorRgb` | `0x000000` to `0xFFFFFF` | Actual RGB color for both generated-image and live vector needles. |
| `screen3Type` | `4` or `8` | Setup option **Screen3 type**. Default 8 retains the original layout and 32px bars. 4 shows slots 1–4 in two rows closer to the center, with 60px value fonts and 48px bars, plus slot 8 centered at the top with a 25px caption underneath (no bar). Hidden slots keep their selections. |
| `screen3BarRadius` | `0` to `20` | Configuration-only corner radius in pixels for both the background and fill of all Screen3 bars. Default 0 (square corners). |
| `mainValuesColor` | `0` to `255` | 0 = white/white; 1–10 = light-to-dark gray (230 to 50); 11–255 = hue selection. Controls main numeric values and the complementary simple-gauge arc value/units caption, not dial ticks. Out-of-range value blinking temporarily overrides the base colors. |
| `simpleGaugeBarShadow` | `0` or `1` | Enables the shadow around the Screen 6 simple-gauge bar. |
| `simpleOverlayImage` | `0` or `1` | Shows the custom `GaugeBG5.png` overlay on the simple gauge when enabled. |
| `limitGaugeValues` | `0` or `1` | Keeps displayed values within the `min` and `max` limits defined in `canmap.conf`. |
| `showNeedle` | `0` to `2` | Main gauge indicator: `0` Arc, `1` Needle and arc, `2` Needle only. |
| `needleType` | `0` to `3` | Needle style: `0` built-in image, `1` custom `Needle.png`, `2` generated and rotated `Vector image`, `3` live-rendered polygon `Vector needle`. |
| `vectorNeedleColor` | `0` to `255` | Vector needle slider position and legacy fallback; `vectorNeedleColorRgb` takes precedence. |
| `Screen0NeedlePivotPoint` | `x,y,radial` | Screen 0 needle pivot and signed radial root offset in pixels. Default: `233,233,57`. |
| `Screen1UpperNeedlePivotPoint` | `x,y,radial` | Screen 1 upper-needle pivot and signed radial root offset in pixels. Default: `233,233,57`. |
| `Screen1LowerNeedlePivotPoint` | `x,y,radial` | Screen 1 lower-needle pivot and signed radial root offset in pixels. Default: `233,233,57`. |
| `Screen2NeedlePivotPoint` | `x,y,radial` | Screen 2 needle pivot and signed radial root offset in pixels. Default: `233,233,57`. |
| `VectorNeedleDimensions` | `base,tip,length` | Global symmetrical vector-needle trapezoid dimensions in pixels. Default: `20,4,154`. Base width: `2`–`200`; tip width: `1`–base width; length: `16`–`400`. |
| `arcType` | `0` or `1` | Arc style on gauge screens 1 to 3: `0` custom image, `1` basic colored arc. |
| `showNumericValues` | `0` or `1` | Show the live numeric value labels on needle screens 1 to 3. |
| `chartUpdateRateMs` | `0` to `100` | Delay between chart updates in milliseconds. Lower values are faster but use more processing power. |

Needle pivot coordinates are absolute display coordinates: X increases to the right and Y increases downward. The radial value positions the inner/root edge of the image or vector needle along its rotation radius. Positive values move the graphic away from the pivot; negative values move its root across the pivot. The four pivot parameters and `VectorNeedleDimensions` are edited directly in `setup.conf` and do not appear on the on-screen Setup page. Pivot values are accepted from `-1024` through `1024`; missing or malformed rows retain their defaults. Vector dimensions apply to all vector needles on screens 0–2, and a malformed dimensions row retains `20,4,154`.

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

Both Arduino and native ESP-IDF builds load these three CAN pins from `setup.conf` before initializing TWAI. In the ESP-IDF build, the corresponding Kconfig values are used only when `setup.conf` cannot be loaded. RX and TX must use different GPIO pins. On Round_base, the SD-card SPI bus is released before TWAI claims a configured pin such as GPIO 41.

## WiFi, ESP-NOW, and debugging

| Parameter | Values | Description |
|---|---:|---|
| `wifiEnabled` | `0` or `1` | Enables the M-Gauge WiFi access point. |
| `wifiPassword` | 8 to 10 characters | Password for the M-Gauge WiFi access point. **Default WiFi password: 12345678**|
| `espNowEnabled` | `0` or `1` | Enables wireless ESP-NOW communication between gauges. |
| `espNowHostMode` | `0` to `2` | ESP-NOW role: `0` Client, `1` Host, `2` One-to-many host. |
| `espNowMacFilter` | `0` or `1` | When enabled, a client accepts normal and pairing traffic only from `espNowHostMac`. If no host has been saved yet, an explicit pairing handshake may establish it. The saved host MAC is shown on the MAC-filter setup page. |
| `espNowPeerRate` | `0`, `1`, `6`, `12`, or `24` | ESP-NOW transmit rate in Mbps. `0` retains the ESP-IDF driver default. The current test default is `6`. |
| `espNowDuplicateFrameIntervalMs` | `5` to `1000` ms | Minimum resend interval for an unchanged CAN frame with the same CAN ID. Changed frames remain immediate. |
| `espNowDiagnostics` | `0` or `1` | Enables a five-second ESP-NOW diagnostic summary containing average/maximum TX queue wait, send-callback latency, average/maximum RX queue wait, current/maximum queue depths, and drop counts. The separate LCD4 UI/render, RGB-copy and telemetry-lock timing test is disabled by default and also requires its compile switch; see [LCD4 timing test](test/LCD4_UPDATE_TIMING.md). These are local processing measurements, not synchronized one-way radio latency. |
| `espNowMGaugeProtocol` | `0` or `1` | M-Gauge CAN transport: `0` sends one CAN frame per ESP-NOW packet; `1` sends up to eight CAN frames per bundle, with a nominal 5 ms collection timeout serviced by the TX worker independently of graphics. Scheduling and radio transmission can add delay. Receivers accept both formats. |
| `espNowHostMac` | MAC address | Saved host address, for example `AA:BB:CC:DD:EE:FF`. This is normally managed automatically. |
| `espNowPeerMacs` | MAC address list | Comma-separated list of ESP-NOW peers. This is normally managed automatically. |
| `telnetEnabled` | `0` or `1` | Enables the decoded CAN-signal and `INTERNAL:` ASCII stream on TCP port 2323. |

The **Web Config Server** switch is controlled from the Gauge Setup screen. It is not currently stored as a separate `setup.conf` parameter.

## Maximum value indicator

`SETTING:maxValueIndicator:0` is **Off** (default), `1` is **Lagging Max**
(the original hold/fade behavior), and `2` is **Max Value** (retain the peak
without fading). All three options are available in setup and Live setup.
A light-blue (`0xADD8E6`), 9px-thick line marks a rising value's peak
once it starts falling: radial on needle screens 0–2 and the simple-gauge arc
(`ui_Screen6`), perpendicular to the bars on `ui_Screen4` in either 4- or
8-value mode. On `ui_Screen6` with only Bar enabled, a horizontal line marks
the main signal's peak across the vertical bar, below the numeric text.
Arc/Both modes retain the arc marker; Horizontal Bar and None have no marker.
Hidden elements do not produce markers.
In Lagging Max mode it holds for five seconds, then fades over 500 ms.
Max Value mode remains fully visible until replaced by a higher peak or reset;
smaller subsequent peaks do not replace it. Peaks are runtime-only, not saved.
Each needle or bar has its own marker and independent hold/fade timer, allowing
both needles or all visible bars to show peaks simultaneously. A higher value
replaces only that element's held peak. Tracking uses the displayed, smoothed
and limited value. Changing a favorite, hiding an element or changing its
geometry resets only that element; screen or mode changes reset all markers. Headless
mode creates no marker graphics.

## CAN control commands

CAN control commands require CAN Bus Link enabled and a hardware CAN filter
that admits the command ID. Standard (11-bit), non-RTR frames with at least two
bytes use byte 0 as the event and byte 1 as its value. Brightness (event `0x01`)
accepts the gauge's own ID as well as its configured gauge-group IDs; screen
change (`0x03`, screen 0–7) and displayed signal (`0x04`, signal slot index)
target the gauge's own ID. For a single gauge at `0x500`, examples are
`0x500: 01 B4` (brightness 180), `0x500: 03 02` (screen 2), and
`0x500: 04 39` (signal 57 in the current screen's first value slot).
Received CAN brightness is not retransmitted onto CAN; an ESP-NOW host can
still synchronize it to clients. Local brightness changes continue to transmit.

## Main value and description positions

Label positioning on screens 0–2 is configured by the eight `Screen*MainValuePosition`
and `Screen*TextLabelPosition` entries listed below. Each pair is the label centre
in the centred 466×466 gauge canvas: X increases rightward, Y downward, and
`233,233` is the centre. LCD4 adds its existing 7px canvas margin on each axis.
The dual-needle screen has independent Upper and Lower entries. Missing entries
use the original layout; invalid pairs are rejected without partially updating
the position. Font/text changes preserve the centre. These options are file-only;
upload `setup.conf` and restart to apply them.

## Color sliders and manual RGB editing

The five RGB settings below use exactly `0xRRGGBB` (for example `0xFF8000`).
They override the remembered slider indices, regardless of file ordering.
Older files without RGB entries retain their palette-derived colors; saving
settings writes the resolved RGB entries. Manual RGB values load unchanged,
even when the secondary color is unlocked.

Click the setup value field for **Simple Gauge Color** or **Main values color**
to freeze the secondary color. The field displays `(#RRGGBB)` in that color;
moving the slider changes only the primary color. Click again to unlock and
immediately restore automatic secondary coloring. Simple gauge uses RGB
inversion; main values use complementary hue, or inverted gray brightness
(white/white at the default). One shared runtime-only lock is released when
another setting is selected or Setup is left, without changing the chosen RGB
colors. The lock is never saved; the five RGB colors use the normal save workflow.

Some other settings may also have the value freeze functionality to allow two values edited from teh same setup page. On **Shift light setpoint** tapping the value field locks teh setup value to allow  **shiftLightOverlimit** to be adjusted with the slider


### Live setup in the browser

Enable **Web Config Server**, open the configuration address shown on the gauge,
then choose **Live setup** in the top header (or open `/live-setup` directly).

<img width="800" height="731" alt="M-Gauge_web_configuration2_larger" src="https://github.com/user-attachments/assets/c0d5d24f-55ca-4453-a745-922d92bad991" />

The separate page mirrors the physical setup dropdown and slider, including
board-specific options, colors, temporary value locks and ESP-NOW pairing.
Browser selection does not move the gauge's selected setup row.
The ESP-NOW section includes **Start Demo / Stop Demo** beside **Start Pairing**, a runtime-only
toggle using the same synthetic CAN generator as the physical Demo switch. In
ESP-NOW Host/One-to-many mode it forwards demo frames to clients, including while
web configuration is open and in headless mode. CAN Bus Link is temporarily
suspended during demo and restored on stopping unless explicitly changed. Demo
does not start automatically after reboot. Generation pauses during file transfers
and OTA; closing the browser does not stop it, so use **Stop Demo** when finished.
Its header uses the same width and Gauge ID/CAN-Bus ID/IP information as the
main configuration page. Both pages use black backgrounds, light bronze/gold
accents and subtle panel shadows. Headers are text-only; boot images remain
available for the gauge's boot splash and custom image uploads.

Changes apply immediately to runtime. **Save settings** persists them atomically;
existing pairing-related automatic saves still apply. Temporary locks are not
saved. Connection-breaking options and destructive actions require confirmation.
Turning Web Config Server Off reboots the gauge as before.

Only one browser can edit live settings at a time. Closing/leaving the page
releases its editor; a disconnected editor expires after 30 seconds. File/image
uploads and OTA temporarily exclude live commands. During web maintenance only
ESP-NOW control/pairing is retained, not normal gauge telemetry rendering.

When switching back to the setup.conf file editor, first save any live changes,
then **LOAD** the file before editing it. SAVE refuses stale revisions, including
changes made on the physical setup page or by pairing. After SAVE, LOAD again
before another file edit. LOAD is refused while live changes remain unsaved.
Hidden text/coordinate/dimension settings still belong
in setup.conf rather than the live dropdown.

Client/one-to-many ESP-NOW pairing uses matching 20-second pairing windows on
both gauges. A client request is accepted only by a one-to-many host whose
pairing window is open; ordinary CAN or brightness traffic cannot establish a
pairing. Disabling ESP-NOW resets `espNowMacFilter`, `espNowHostMac`, and
`espNowPeerMacs`.

### Demo mode

Test M-Gauge without a live CAN connection using simulated driving data. Values accelerate, decelerate, coast and change direction smoothly instead of repeatedly sweeping between minimum and maximum.
Demo data can also be sent over ESP-NOW to test wireless clients. Start or stop Demo Mode from the gauge’s setup screen or the web-based Live setup page—even on a headless gateway. 





## Shift light, alarms, and status

| Parameter | Values | Description |
|---|---:|---|
| `revLimitSourceIndex` | `0` to `83` (`94` on native Round_waveshare) | Signal number from `canmap.conf` used to control the shift light. This is normally the RPM signal. |
| `shiftLight` | signal value | Value at which the shift light becomes active. Its unit follows the selected shift-light signal. |
| `shiftLightStep` | signal value | Difference between the shift-light points of consecutive linked gauges. Use `0` for the same point on every gauge. |
| `shiftLightOverlimit` | scaled signal value (-1000000 to 1000000) | Independent red-background threshold for Shift Light Src. For RPM, `7000` means 7000 RPM; decimal values are accepted. Does not change the signal's alarmLimit or alarm ticks. If absent, initially uses the selected signal's alarmLimit; once resolved, normal settings saves persist it. |
| `demoBoostLimitX10` | kPa | MAP pressure that activates the boost warning. The legacy setting name is retained for setup.conf compatibility; for example, `150` means `150 kPa`. |
| `displayErrors` | `0` or `1` | Shows ECU error messages when enabled. |
| `displayStatus` | `0` or `1` | Shows active status-bit messages when enabled. |
| `alarmMonitorEnabled` | `0` or `1` | Monitors the signals listed in `ROLE:alarmMonitor` and warns when a limit is exceeded. |

On **Shift Light Setpoint**, tap the value to lock the setpoint (parentheses).
The slider then edits **Shift Light Overlimit**, displayed as `R=` in the limit
preview, without changing the setpoint or step. Tap again to edit the setpoint.
Leaving the option or Setup clears this temporary lock without changing either
threshold. The slider uses the selected source's range and adjustment increments;
the configuration file accepts fractional values directly. Save settings normally
to retain either threshold. Setup and button screens remain excluded from
shift-light background changes.

## Internal sensor settings

| Parameter | Values | Description |
|---|---:|---|
| `accelerometerEnabled` | `0` or `1` | Enables the board motion sensor for accelerometer and gyroscope values. |
| `dragMode` | `0`, `1`, or `2` | Native ESP-IDF Round_waveshare only: disables Drag mode, selects 1/8 mile, or selects 1/4 mile. Drag mode samples the QMI8658 independently of `accelerometerEnabled`. |
| `dragCalibrationFactor` | `0.800` to `1.200` | Drag-only acceleration multiplier, rounded to `0.001` steps. Default `1.000`; editable in setup.conf, including through web configuration. Captured when the run is armed. |
| `internalInputsEnabled` | `0` or `1` | Enables user-defined GPIO, I2C, and supported board-microphone inputs from `INTERNAL:` rows in `canmap.conf`. |
| `microphoneFrequencyFilter` | `0` to `4` | Native Round_waveshare microphone band: Full range, Bass, Midrange, Treble, or Custom. |
| `microphoneLowCutHz` | `1` to `10000` | Custom-mode high-pass cutoff. Default `50`; ignored by preset modes. |
| `microphoneHighCutHz` | `1` to `10000` | Custom-mode low-pass cutoff. Default `10000`; ignored by preset modes. |

## Screen, selection, and favorite rows

These rows do not begin with `SETTING:`.

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
| `smoothingMs` | `0` to `60000` | Optional display-smoothing time constant in milliseconds. Missing or `0` disables smoothing. Also supported on `INTERNAL:` rows. |
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

Optional `link=` on a `UNIT:` rule selects another SIGNAL slot for the comparison:

```ini
UNIT:57,raw>10,text=Balls Failure,link=1
```

This changes signal 57's caption when signal 1's stored raw value exceeds 10;
signal 57's numeric value is unchanged. Without `link=`, the rule checks its own
signal as before. `LINK=` is also accepted. The link is a decimal SIGNAL slot,
not a CAN ID or another UNIT rule. Forward references are supported; invalid or
undefined links are ignored with a configuration warning. Matching rules retain
file order (first match wins). Linked rules use the source's current stored value,
including its initial value before reception; they do not add a freshness timeout.

## ECU error descriptions

An `ERROR:` row gives a readable message to an ECU error code.

```text
ERROR:12,text=Coolant temperature sensor fault
```

| Field | Description |
|---|---|
| Number after `ERROR:` | ECU error code from `0` to `254`. |
| `text` | Error message shown to the user. Use `|` where a line break is wanted. |

M-Gauge loads up to 96 `ERROR:` descriptions. Description text may contain commas and is limited to 95 characters.

Error messages are shown only when `SETTING:displayErrors:1` is enabled and the error role signals are configured.

## Optional display smoothing

Append `smoothingMs=<milliseconds>` to any `SIGNAL:` or `INTERNAL:` row:

```text
SIGNAL:73,id=0x52A,startBit=48,bitLength=16,endian=little,signed=0,factor=1,offset=0,scaling=10,min=0,max=100,name=L|Fuel Tank,smoothingMs=3000
INTERNAL:28,source=I2C,device=INA226,address=0x40,mode=voltage,factor=1,offset=0,scaling=1000,min=0,max=36,name=V|Supply Voltage,smoothingMs=200
```

The time-aware exponential filter smooths numeric displays, needles, arcs, bars,
and chart samples on all six gauge pages. It does not slow screen refreshing.
`200` gives light smoothing; `3000` is a starting point for fuel level. After a
step change, a 3000 ms filter reaches about 63% in 3 seconds and 95% in 9 seconds.
It continues approaching the target between received samples, without overshoot.

Raw stored values, alarms (including numeric alarm blinking), shift lights,
Telnet, ESP-NOW, and `VALUE:`/`UNIT:` conditions are unchanged. Text overrides
such as `N` display immediately; they are not interpolated. Keep gears, switches,
and other discrete signals unsmoothed. `BIT:` indicators are unaffected.

Missing or zero smoothing uses the original display path with no filter-state
allocation or filtering calculations. Positive durations allocate a small state
per signal, preferring PSRAM. The first successful sample displays immediately;
failed reads are not treated as zero. Filters continue while their signals are
hidden, and changing screens does not restart them. Reloading `canmap.conf`
clears the old filters; re-enabling a physical INTERNAL input starts from its
first fresh sample. Invalid durations (including negative, fractional, or values
above 60000) are rejected. Smoothing is configured in `canmap.conf`, not as a
global setup-page option.

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

## Drag performance meter (native Round_waveshare)

The ESP-IDF Round_waveshare build provides `SETTING:dragMode:1` for a 1/8-mile run and `SETTING:dragMode:2` for a 1/4-mile run. It is disabled with `0`. The trigger role may point to either a decoded bit or signal; the default is clutch-pedal BIT 9:

```text
ROLE:dragModeTrigger=BIT:9
```

The source must first be active and then transition from active to inactive. After that falling edge, the meter waits for a confirmed acceleration increase before starting its clock and showing `RACE!`. Timing-line crossings are interpolated between accelerometer samples. Results remain available until another trigger or a Drag mode change.

`SETTING:dragCalibrationFactor:1.000` leaves the calculations unchanged. A value
of `1.050` increases longitudinal acceleration by 5% before speed/distance
integration; `0.950` reduces it by 5%. The real elapsed-time clock, launch
threshold, baseline, launch direction, and other accelerometer consumers are
unchanged. A run keeps the factor captured at arming, even if the setting later
changes. Invalid/nonfinite/out-of-range file values are rejected; a missing
setting defaults to `1.000`. SD logs record the captured factor in the summary
header, retain the original XYZ and `longitudinal_g` measurements, and add
`calibrated_longitudinal_g` alongside the calibrated performance results.

Calibrate against multiple reference passes and intermediate times. A single
multiplier cannot remove time-varying bias, vehicle pitch, or launch-timing
differences. Finish speeds are instantaneous estimates, not speed-trap averages.

The native Waveshare signal table reserves these computed values:

| Signal | Value | Scaling |
|---:|---|---:|
| `84` | 60ft Time (s) | `1000` |
| `85` | 330ft Time (s) | `1000` |
| `86` | 990ft Time (s) | `1000` |
| `87` | 1/4 Mile Speed (km/h) | `10` |
| `88` | 1/4 Mile Time (s) | `1000` |
| `89` | 1/8 Mile Speed (km/h) | `10` |
| `90` | 1/8 Mile Time (s) | `1000` |
| `91` | Time from start (s) | `1000` |
| `92` | Distance from start (m) | `100` |

These are calculated signals, not physical `INTERNAL:` sensor channels. Their `INTERNAL:` rows in the supplied `canmap.conf` provide display names, units, ranges, and scaling only. Accelerometer-only distance and speed are estimates: mounting alignment, vehicle pitch, vibration, sensor bias, wheelspin, and track gradient affect accuracy, so this feature must be calibrated and compared with track timing before treating its results as authoritative.

## Internal GPIO inputs

An `INTERNAL:` row with `source=gpio` displays a digital or analog GPIO input as a gauge signal.

```text
INTERNAL:28,source=gpio,pin=1,mode=analog_mv,invert=0,factor=1,offset=0,scaling=1000,min=0,max=3.3,alarmLimitMin=0,alarmLimit=3.3,alarmSource=0,name=V|Sensor Input
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

Only GPIO `1`, `2`, `3`, and `5` in that allowlist are ADC-capable on ESP32-S3. Use the other allowed pins only with a digital mode.

> Check the board wiring before selecting a pin. The user is responsible for choosing a suitable digital or analog pin and keeping the input voltage within the ESP32 electrical limits.

## Internal I2C voltage input

With `SETTING:internalInputsEnabled:1`, boot performs one general I2C0 address
scan immediately before the INA226 identification probe. Serial output lists
the board's SDA/SCL pins, responding addresses (`0x08` through `0x77`), and a
summary. An acknowledged address indicates presence, not a confirmed device
model. The scan reuses the existing bus without register reads/writes, stops
on a bus fault or its one-second time budget, and is skipped if the bus is
unavailable. Internal Inputs disabled means no general boot scan. The scan
is not repeated during normal operation or when enabling inputs later.

An `INTERNAL:` row with `source=I2C` can read the bus-voltage register of an INA226 connected to the LilyGo QWIIC connector. The QWIIC bus uses GPIO `7` for SDA and GPIO `6` for SCL and is shared safely with the touch controller.

```text
INTERNAL:28,source=I2C,device=INA226,address=0x40,mode=voltage,factor=1,offset=0,scaling=1000,min=0,max=36,alarmLimitMin=0,alarmLimit=15,alarmSource=0,name=V|Supply Voltage
```

| Field | Values | Description |
|---|---:|---|
| `source` | `I2C` | Selects an I2C device as the value source. |
| `device` | `INA226` | Selects the supported monitor. `NA226` is accepted as an alias. |
| `address` | `0x40` to `0x4F` | Seven-bit INA226 I2C address. Default is `0x40`. |
| `mode` | `voltage` or `bus_voltage` | Reads the INA226 bus-voltage register. |
| `factor` | number | Multiplies the millivolt reading. Usually `1`. |
| `offset` | number | Amount added after the factor. Usually `0`. |

The raw internal value is integer millivolts. Use `scaling=1000` to display volts. Enable `SETTING:internalInputsEnabled:1` to sample the device. Up to six GPIO and I2C inputs can be configured in total. INA226 current, shunt-voltage, and power measurements are not supported yet.

An `INTERNAL:` definition replaces CAN decoding for the same signal number. Define `source` before the other input fields on that row.

### Waveshare microphone intensity and frequency

The native ESP-IDF `Round_waveshare` build can combine its two onboard ES7210 microphones into a logarithmic sound-intensity signal. The stored value is tenths of a percent, so use `scaling=10` for a displayed range of `0.0` to `100.0 %`:

```text
INTERNAL:93,source=microphone,factor=1,offset=0,scaling=10,min=0,max=100,alarmLimitMin=0,alarmLimit=100,alarmSource=0,name=%|Sound Intensity
INTERNAL:94,source=microphoneFrequency,factor=1,offset=0,scaling=1,min=0,max=10000,alarmLimitMin=0,alarmLimit=10000,alarmSource=0,name=Hz|Dominant Frequency
```

Enable `Internal Inputs`, then select either microphone signal like any CAN
signal. `Sound Intensity` is the filtered dual-microphone level.
`Dominant Frequency` is a 1024-sample Hann-window FFT result, refreshed at
about 10 Hz and constrained to the selected filter band. Below the frequency
detector's silence threshold it reports `0 Hz`.

The filter presets are Full range `50-10000 Hz`, Bass `50-250 Hz`, Midrange
`250-2000 Hz`, and Treble `2000-10000 Hz`. Custom uses the two cutoff settings;
normally the high cutoff should be at least the low cutoff. `factor` and `offset` remain available
for installation-specific calibration.

Microphone capture is demand-driven. The codec and I2S capture start only when
a microphone signal is on the active gauge screen, is the Shift Light Src, is
an enabled alarm-monitor signal, or a connected CAN ASCII Stream client needs
INTERNAL values. Frequency analysis runs only while `Dominant Frequency` is
demanded. After 1.5 seconds without a consumer, the codec is powered down.
Merely listing the signal in Favorites does not activate sampling until that
favorite is selected for display.

The current `Waveshare_LCD4` software profile targets `ESP32-S3-Touch-LCD-4` V4 hardware, whose published pin map does not expose an ES7210/audio input. Do not copy the incompatible `Touch-LCD-4B` microphone pins into that profile; add a separate 4B board target if that is the fitted LCD4 hardware.

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

**Short teaser video below**

[VIDEO of M-Gauge features](https://flic.kr/p/2snAxms)



### Headless CAN/ESP-NOW gateway

`SETTING:headlessMode:0` is the default. Set it to `1` in `setup.conf`, save and
reboot to run without a display. The mode is latched at boot: a web file reload
does not initialize graphics while the gauge is running.

Headless mode skips display/touch initialization, LVGL, draw buffers, decoded
images, boot splash and graphics updates. SD boot synchronization reads only
`canmap.conf`, not images. Enabled INTERNAL sensors can initialize their I2C bus
without initializing touch. Compiled graphics code remains in the firmware.

The Wi-Fi AP and configuration server remain available even with `wifiEnabled:0`
and `webServerEnabled:0`. Use the existing Wi-Fi password and connect to
`M-Gauge-AP_<gaugeId>` at `http://192.168.0.<100 + gaugeId>` (host number clamped
to 100–199). The Live setup Web Config Server control is fixed On in headless
mode. CAN decoding/forwarding and ESP-NOW continue while browsing, unlike normal
display-mode web maintenance. Firmware updates pause telemetry; configuration
reloads and radio changes can briefly interrupt it.

Configure `canBusLink:1`, the correct CAN pins/speed/filter, `espNowEnabled:1`
and `espNowHostMode:1` (Host) or `2` (One-to-many). Existing pairing, peer rate,
MAC filtering and bundled-protocol settings are unchanged; headless mode does
not automatically change the radio role or enable CAN. Keep `wifiEnabled:0` if
you want to retain the usual STA ESP-NOW MAC while using the temporary web AP.

`/health` reports `headless` (current boot mode) and `headlessModePending`
(saved/configured mode differs from this boot). To restore graphics, save
`SETTING:headlessMode:0` and power-cycle/reboot. A reboot is required; turning
off the web server is intentionally unavailable while running headless.





