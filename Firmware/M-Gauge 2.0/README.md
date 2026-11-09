# M-Gauge 2.0 — More than a gauge

M-Gauge 2.0 brings together the new and expanded features developed since version 1.1.0: deeper visual customization, onboard sensor inputs, a redesigned browser interface, display-free gateway operation, and more capable wireless gauge networks.

Whether you want one personalized instrument or several linked displays sharing a single CAN connection, this release gives you more control over how your M-Gauge system looks and works.

## Native ESP-IDF and expanded hardware support

The firmware now runs natively on ESP-IDF, with dedicated display and touch support for:

- LilyGo 1.75-inch round AMOLED.
- Waveshare 1.75-inch round AMOLED.
- Waveshare 4-inch LCD, using the supported LCD4 board profile.

Hardware-specific features depend on the fitted sensors and selected board profile. In particular, microphone input and the drag performance meter are currently available on the Waveshare round build, not every supported board.

## Make the display your own

- **Independent colors:** customize main numeric values, normal dial ticks, alarm-limit ticks, simple-gauge bars and arcs, and vector needles. Use the setup color sliders or edit the individual RGB hex colors in `setup.conf`.
- **Secondary-color locking:** tap the setup value to freeze a secondary color while adjusting the primary color. Keep complementary colors, choose matching colors, or create your own combination.
- **Custom needle styles:** choose a built-in or custom image, a generated **Vector image**, or a live-rendered **Vector needle**. Configure vector needle base width, tip width and length, plus individual needle pivot positions and radial offsets.
- **Adjustable layouts:** reposition the main values and description labels on needle screens, including independent upper and lower labels on the dual-needle screen.
- **More display choices:** select four- or eight-value dashboard layouts, adjust bar corner radius, use a horizontal simple-gauge bar, toggle its shadow, or hide numeric values on the needle screens.
- **Custom artwork and animation:** use custom backgrounds, arcs, needle and button images. PNG images remain supported, with animated GIF support added for boot and dedicated button images. Boot animation plays once; button GIF animation runs while the button is pressed.
- **Peak markers:** choose **Off**, **Lagging Max** for a held peak that fades away, or **Max Value** to retain the peak without fading. Multiple needles and bars track their peaks independently.

Some advanced layout and geometry options are edited through `setup.conf`; everyday settings remain accessible from the gauge and Live setup.

## A new web configuration experience

The browser interface has a black-and-gold/bronze design, responsive controls, and firmware-version information in the page headers.

The main configuration page lets you:

- Load, edit and save `canmap.conf` and `setup.conf`.
- Upload custom images, with format, dimension and file-size checks before activation.
- View and download supported drag-performance SD logs.
- Install application-only OTA firmware updates while preserving configuration and LittleFS files.

### Live setup — adjust the gauge from your browser

A separate **Live setup** page mirrors the supported setup dropdown and slider controls. Change settings in real time, preview color selections, use temporary value locks, and save your changes without editing configuration text.

The ESP-NOW section shows the configured mode, saved host, registered clients and pairing status. Enable or disable ESP-NOW, change its role, start pairing, and start or stop demo mode remotely.

Live setup provides live configuration controls and device status; it is not a streamed mirror of the gauge display or a live telemetry chart.

## Headless mode — a dedicated CAN-to-wireless gateway

M-Gauge can now operate without a display as a **headless node**. One unit connects to the vehicle CAN bus and forwards data to the gauges in your ESP-NOW network.

Headless mode skips display and touch initialization, LVGL graphics, decoded images and the large graphics buffers, freeing RAM for gateway operation. Its Wi-Fi configuration access and web server remain available, so you can manage the node through the main configuration page and Live setup.

Normal CAN processing and ESP-NOW forwarding continue while browsing in headless mode. Remote pairing, configuration and demo-data generation make it possible to set up and test a display-free installation. Firmware updates pause telemetry, and configuration or radio changes can briefly interrupt communication.

Enable `SETTING:headlessMode:1` in `setup.conf` and reboot. Set it back to `0` and reboot to restore display operation.

## Expanded ESP-NOW networking

**One-to-many mode** lets one CAN-connected host supply several wireless M-Gauge clients, reducing the CAN wiring needed behind the dashboard.

- Explicit pairing and saved host/client registrations simplify building a network that reconnects after reboot.
- Client host-locking and optional host-MAC filtering help prevent accepting data from an unintended gauge.
- Active-client tracking avoids treating every saved peer as currently connected.
- Optional **M-Gauge frame bundling** carries several CAN frames in one ESP-NOW packet for more efficient transport.
- Configurable peer rate, duplicate-frame interval and optional queue/latency diagnostics support installation-specific tuning.
- Demo generation and forwarding run independently of the host graphics loop, so demanding host screens do not directly control the demo transmission schedule.

**Security note:** pairing, peer registration and MAC filtering provide basic separation between gauge networks, not cryptographic security. The current ESP-NOW transport is unencrypted; do not treat it as confidential or protected against a deliberately spoofed sender.

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

## Updated CAN buttons and visual status indicators

Alongside the dedicated CAN-button screen, M-Gauge now supports **four additional customizable image buttons**, each with its own CAN event ID, across the supported builds.

Use them as buttons, status indicators, or both:

- Send a configured CAN event when pressed, including from an ESP-NOW client through its host.
- Link each image to a `BIT:` status using visibility, flashing or opacity behavior.
- Show actionable buttons dimmed when inactive, with a brighter pressed/active state.
- Select status-screen, corner-button or supported gauge-overlay placement. Needle-screen overlays follow an upper arc inside the dial ticks; setup remains free of these controls.

Updated CAN control handling also supports brightness, screen-selection and displayed-signal commands addressed to the gauge itself, while avoiding brightness retransmission loops.

## More flexible signals, warnings and testing

- Optional **`smoothingMs`** on CAN and internal signals steadies noisy values such as fuel level. Missing or zero leaves smoothing disabled, and raw telemetry remains unchanged.
- Shared value formatting improves consistent scaling, signed values and decimal precision across screens.
- Conditional **`VALUE:` and `UNIT:`** text overrides, linked unit conditions and configurable **`ERROR:`** descriptions make status messages more adaptable.
- Expanded alarm/status displays, selectable shift-light sources and a separately adjustable **Shift Light Overlimit** threshold give more control over warnings.
- Supported internal inputs include GPIO digital/analog measurements and external **INA226 bus-voltage monitoring**, with user-defined scaling.
- A driving-like demo replaces repetitive min/max sweeps with smooth acceleration, deceleration, coasting and partial-range reversals. Demo data can also be sent to wireless clients, including from a headless node.

**M-Gauge 2.0 combines a customizable instrument, a sensor display, a wireless CAN gateway and a browser-managed gauge network in one firmware platform.**
