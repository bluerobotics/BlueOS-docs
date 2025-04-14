+++
title = "Advanced Usage"
description = "BlueOS advanced usage documentation."
date = 2024-09-26T16:00:00+11:00
template = "docs/page.html"
sort_by = "weight"
weight = 30
draft = false

[extra]
lead = ''
toc = true
top = false
link_base = "https://github.com/bluerobotics/BlueOS/tree/master/core"
+++

## General Information

This documentation page is for detailed information about the services and pages
in the BlueOS web interface. For a higher level list and direct comparison of
features see the [Overview](../overview/#feature-comparison) instead.

### Pirate Mode

The default BlueOS interface is simplified, and shows only the major tools that
most people are likely to find useful. Full functionality is available via
"Pirate Mode", which can be enabled from the [header bar](#display-mode-management).
Note that Pirate Mode is advanced/development mode, and should be used with care.

This documentation by default shows the full functionality interface, to provide
an overview of _all_ functionality instead of a limited subset, but if you're only
interested in the basic functionality you can click the button below:

<div style="text-align:center;padding-bottom:15px;">
<!-- functionality code at bottom of file -->
<button onclick="toggle_advanced()" id="pirate_toggle">
    hide advanced functionality
</button>
</div>

For clarity of this documentation, any pages that are extended by (or only available
in) Pirate Mode are shown in [dark mode](#display-mode-management), and described
with grey text.

### Safe Mode

Operating a vehicle involves some risks to both the vehicle and the operator. When
BlueOS detects that the vehicle is armed it engages safe mode, which requires explicit
confirmation to access functionality like BlueOS and autopilot firmware updates.

{{ easy_image(src="safe-mode", width="400") }}

### Interface Overview
{{ service(service="blueos-frontend", port=80) }}

When you first open BlueOS, you'll see a window like the following:
{{ easy_image(src="interface-overview", width=600) }}

#### Header: Indicators and BlueOS Configuration

On the left side of the header there is space for widgets, which can be accessed by
right clicking and selecting the desired widgets to display. Widgets can be reordered
by clicking and dragging.

There are currently widgets available for displaying the CPU and Disk (storage) usage
as percentages, as well as local network usage, which are periodically updated during
operation:

{{ easy_image(src="widgets", width=350) }}

There is also a prominent indicator when the vehicle is in [Safe Mode](#safe-mode):

{{ easy_image(src="safe-mode-widget", width=100) }}

---

On the right side of the header you'll find:
##### Notifications

{{ easy_image(src="notifications", width=350, center=true) }}

- Press the broom to clear notifications
- Press the gear to toggle showing old messages

##### Wired network management (ethernet / USB-OTG)
{{ service(service="Cable Guy", port=9090, link="/services/cable_guy", based=true) }}

An ethernet connection can generally be accessed through the
[http://blueos-ethernet.local](http://blueos-ethernet.local) mDNS address.

When configuring a wired interface, choose between:
- A static IP
- A dynamic IP
- A DHCP server
   - Serves the lease range 101-200 `(New in 1.4)`

It is possible to have multiple connections per interface type.

{{ easy_image(src="ethernet", width=350, center=true) }}

{{ easy_image(src="usb-otg", width=350, center=true) }}

##### Wifi + Hotspot network management
{{ service(service="Wifi Manager", port=9000, link="/services/wifi", based=true) }}

- Choose a wifi network to connect to
   - The BlueOS web interface is accessible via
   [http://blueos-wifi.local](http://blueos-wifi.local) when
   connected to the same wifi network as your device (including a mobile phone)

{{ easy_image(src="wifi", width=350, center=true) }}

- Forget, connect to, or a force a new password for a saved network

{{ easy_image(src="wifi-example", width=250, center=true) }}

- Configure or turn on/off the BlueOS wireless hotspot, or display a QR code to
easily connect to it from a phone
   - The hotspot SSID is named `BlueOS (******)` by default, where the asterisk
   field varies for each system
      - The default password for the hotspot is `blueosap`
   - The BlueOS web interface is accessible via
   [http://blueos-hotspot.local](http://blueos-hotspot.local) and
   [http://192.168.42.1](http://192.168.42.1) when your device is connected to
   the BlueOS hotspot network

{{ easy_image(src="hotspot-example", width=250, center=true) }}

##### Internet Status and Management

- See whether the vehicle is connected to the internet (updates every 20 seconds)

{{ easy_image(src="internet", center=true, width=50) }}
{% pirate() %}
- Configure network priority ordering
    - Determines which network interface is used for internet connection
    - Displays internet availability on each network
    - Generally `wlan0` should be at the top (for internet via wifi)
    - Move `eth0` to the top if using internet passthrough via the tether
{% end %}
{{ easy_image(src="internet-network-priority", class="pirate", center=true, width=400) }}
{% pirate() %}
- View and configure DNS name servers
{% end %}
{{ easy_image(src="internet-dns-config", class="pirate", center=true, width=400) }}

##### Display Mode Management

{{ easy_image(src="display-mode", width=250, center=true) }}

- [Pirate mode](#pirate-mode) can be toggled via the happy robot /
skull-and-crossbones icon
   - Access or hide advanced functionality
   - **Advanced users only** - pirate mode is not recommended for normal use
- Light and dark display modes can be toggled between with the sun / moon icon

##### System status

{{ easy_image(src="system-status", width=250, center=true) }}

- Heartbeat icon pulses with vehicle heartbeat, and goes red if heartbeat is lost
   - On click shows onboard computer temperature, voltage, and current usage
- GPS icon displays the number of visible satellites if a GPS is detected
   - On click shows the current GPS position estimate, and some GPS health/status values
   - Can show status for two connected GPS systems, and GPS-based yaw if both have valid position fixes `(New in 1.4)`
{{ easy_image(src="gps-status", width=300, center=true) }}

- Additional warning icons appear if a problem is detected on the onboard computer:
   - High disk usage
   - CPU overheating
   - CPU throttling
   - CPU under voltage
   - Connected wirelessly (instead of through a tether)
   - BlueOS cannot connect to its host computer

#### Sidebar

The burger menu at the top left of the header opens up the sidebar, for
conveniently accessing available pages, tools, and services. When the page
is wide enough, the sidebar automatically stays open.

{{ simple_pirate_image(src="sidebar", width=200) }}

- The theme content at the top [is configurable](#theme-content)
{% pirate() %}
- [The development documentation](@/development/extensions/index.md#web-interface-http-server)
specifies the requirements for a service page to appear in the sidebar
{% end %}

##### BlueOS Settings

{{ easy_image(src="settings", width=300, center=true) }}

- Reset BlueOS settings
   - Remove existing camera/endpoint/bridges configuration
- Remove log files from BlueOS services (to reduce space usage on the SD card)
- Download log files from BlueOS services to report a problem
  - Old logs are aggregated and compressed with GZip, and automatically deleted if the space runs out `(New in 1.2)`
- Re-enable the [configuration wizard](../getting-started#wizard)

##### Power

{{ simple_pirate_image(src="power", width=300, center=true) }}

- Power off
   - Shut down onboard computer
   - Recommended before turning off vehicle power
- Reboot
   - Reboot onboard computer
{% simple() %}
- Soft restart
   - Do a software-restart of BlueOS (without restarting the computer)
   - Generally sufficient for most 'reboot' requirements
{% end %}
{% pirate() %}
- Restart core container (same as "Soft restart")
   - Restart the core BlueOS docker container
   - Generally sufficient for most 'reboot' requirements
{% end %}

##### Feedback

{{ easy_image(src="feedback", width=300, center=true) }}

Submit feedback about BlueOS via:
- [Issues on the GitHub repository](https://github.com/bluerobotics/BlueOS/issues)
   - allows easily tracking changes,
and notification when complete/fixed
- Report form submissions
   - Can be submitted at any time, and get sent to the developers when the
     [control station computer](@/integrations/hardware/required/control-computer/index.md)
     next connects to the internet
- Posts on [the Blue Robotics forum](https://discuss.bluerobotics.com)
   - allows easy discussion with the community

### Dashboard
The "Dashboard" page provides an overview of the available pages. In future it
will provide an overview of the vehicle state and main configuration options.
Click the BlueOS logo to return to the dashboard at any point.

{{ simple_pirate_image(src="dashboard", width=600) }}

## BlueOS Service Pages
Pages are sorted alphabetically.

### Autopilot Firmware
{{ service(service="ArduPilot Manager", port=8000, link="/services/ardupilot_manager", based=true) }}

The Autopilot Firmware page provides basic information about the active
autopilot, along with options to:
{% pirate() %}
- Change board (select a connected board, or run an
[SITL](https://www.ardusub.com/developers/sitl.html) simulation)
- Start the autopilot
- Stop the autopilot
{% end %}

- Restart the autopilot
- Update the firmware
   - [ArduPilot](https://ardupilot.org) family of [firmwares](https://firmware.ardupilot.org) only
   - Choose firmware to install
      - Select from the online repository
         - Select vehicle type (Sub / Rover / Plane / Copter)
         - Select desired release and stability level
            - Stable - A production-ready release.
               - The latest Stable is recommended for most users.
               - e.g. `Stable-4.1.1`
            - Beta - In-testing release, with new features and improvements, aiming
            to become stable. May have bugs.
            - Dev - Development branch, with all the newest features. Intentionally
            unstable (changes quickly), and possible untested/dangerous.
      - Upload a custom firmware file from the surface computer
      - Restore the default (ArduSub) firmware for the connected flight controller
   - Flash firmware onto a connected compatible
   [flight controller board](@/integrations/hardware/required/flight-controller/index.md)

{{ simple_pirate_image(src="firmware", width=600) }}

{% pirate() %}
Systems with a
[Navigator](https://bluerobotics.com/store/comm-control-power/control/navigator/)
flight controller also have the option to configure serial-compatible ports from
the onboard computer (including USB ports) as serial ports accessible to the
autopilot.
{% end %}

{{ easy_image(src="firmware-serial", class="pirate", width=600) }}

{% note() %}
It's highly recommended to **flash the default parameters** for the vehicle when a **firmware downgrade or upgrade** is done.
Check the [vehicle setup](#vehicle-setup) for more information.
{% end %}

### Autopilot Parameters
`New in 1.1`

The Autopilot Parameters page allows checking and changing the autopilot's configuration.

- Includes fuzzy searching of names and descriptions, to help find relevant parameters
- Parameter descriptions can be overridden by including a `metatdata_override.json` file
in the `userdata` folder in the [File Browser](#file-browser) `(New in 1.4)`
   - Parameter files can be generated from an ArduPilot firmware using
     [this tool](https://github.com/ArduPilot/ardupilot/blob/master/Tools/autotest/param_metadata/param_parse.py)
- Allows loading parameters from a file, and saving the current parameters to a file

{{ easy_image(src="parameters" width=600) }}

{% pirate() %}
### Available Services

The Available Services page provides developer access to the underlying http
server interfaces of the services upon which BlueOS is based. Each service is
listed with
- the port it is served at
- a meaningful name
- a web-page link using the active
[network configuration](../getting-started/#interface-access)

and where relevant
- its API documentation (in a live-testable form)
- the current API version

The individual services are documented [in the development documentation](@/development/core/index.md#services).
{% end %}
{{ easy_image(src="available-services", width=600, class="pirate") }}

{% pirate() %}
### Bag Editor
{% end %}
{{ service(service="Bag of Holding", port=9101, link="/services/bag_of_holding", based=true) }}
{% pirate() %}

The Bag Editor is a helper service for advanced users, which allows modifying the
database used to handle frontend interface changes.
{% end %}
{{ easy_image(src="bag-editor", width=600, class="pirate") }}

### BlueOS Version

{{ service(service="Version Chooser", port=8081, link="/services/versionchooser", based=true) }}

The Version Chooser is a major component in the robust backbone of BlueOS. It
runs independently from the main interface, and is monitored such that if it
somehow fails a backup version will be run in its place.

{{ simple_pirate_image(src="version-chooser", width=600) }}

- The simplified interface provides an easy way to update to the latest version
that is [as stable or more stable](../overview/#release-types) than the currently installed version
{% pirate() %}
- The full interface supports easily changing forwards _and backwards_ between
versions
   - Previously-installed versions are kept locally on the device, unless
   manually deleted, which provides an easy route for roll-backs to undesired
   changes (e.g. during development)
- Allows updating the [bootstrap image](@/development/bootstrap/index.md) to match the current version
- Allows loading remote versions (including from custom docker-hub repositories)
- Allows manually uploading docker images from the surface computer
- If an undetected failure somehow occurs in BlueOS (or if a broken version gets
installed) it's possible to easily roll back to a working version from
   - on the device
   - manual upload, or
   - downloaded from the internet
- If necessary, the underlying service can be accessed directly
   - e.g. [http://blueos.local:8081](http://blueos.local:8081)
{% end %}

{% pirate() %}
### File Browser
{% end %}
{{ service(service="File Browser", port=7777, link="https://github.com/filebrowser/filebrowser") }}
{% pirate() %}
The File Browser allows viewing, editing, downloading, and uploading BlueOS files.
{% end %}
{{ easy_image(src="file-browser", width=600, class="pirate") }}
{% pirate() %}
Vehicles using a Navigator flight controller board with a recent ArduPilot firmware can access Lua
drivers and scripts at `configs/ardupilot/firmware/scripts`. `(New in 1.2)`
{% end %}

### Log Browser
{{ service(service="UAV LogViewer", link="https://ardupilot.org/copter/docs/common-uavlogviewer.html") }}

- Allows downloading telemetry `.bin` logs from Linux-based autopilots
   - Set the `BRD_RTC_TYPES` autopilot parameter to include `MAVLINK_SYSTEM_TIME`
   so the filenames use timestamps
- Can stream logs from external flight controllers (e.g. Pixhawks) if the
`LOG_BACKEND_TYPE` autopilot parameter is set to `MAVLink`
   - [May be inconsistent](https://github.com/bluerobotics/BlueOS/issues/457)

{{ easy_image(src="log-browser", width=600) }}

- Press the green play button to access the built in Log Viewer, to visualise and
analyse vehicle telemetry (including position if a positioning system is equipped)
{{ easy_image(src="log-viewer", width=600) }}

{% pirate() %}
### MAVLink Endpoints
{% end %}
{{ service(service="ArduPilot Manager", port=8000, link="/services/ardupilot_manager", based=true) }}
{% pirate() %}

The MAVLink Endpoints manager allows configuring the serial, UDP, and TCP
endpoints for MAVLink-based services and programs to access.
{% end %}
{{ easy_image(src="mavlink-endpoints", width=600, class="pirate") }}
{% pirate() %}
- The default MAVLinkRouter can be switched to instead use:
   - [MAVP2P](https://github.com/bluenviron/mavp2p) `(New in 1.2)`
      - This may use more CPU, so is only recommended if your system is having frequent "GCS Heartbeat Lost" errors
      - Does not yet support generating telemetry log files
   - [MAVLinkServer](https://github.com/bluerobotics/mavlink-server) `(New in 1.4)`
      - All messages are forwarded to all clients (does not attempt to filter by component/target IDs)
         - Endpoints do not have behavioural differences (e.g. UDP vs TCP, client vs server)
      - Allows websocket and cross-websocket communication, with a
        [MAVLink2REST](https://github.com/mavlink/mavlink2rest?tab=readme-ov-file#api)-compatible API
      - Logs all messages passed through, instead of just those from the vehicle
         - Logs can be visualised and downloaded through the [Log Browser](#log-browser)
      - Provides a detailed debugging interface (accessed via [Available Services](#available-services)):
{% end %}
{{ easy_image(src="mavlink-server", width=550, class="pirate") }}
{% pirate() %}
- Endpoints intended for internal BlueOS operations are configured to the
loopback IP `127.0.0.1`
- Server endpoints for external use are configured to the localhost IP
   - e.g. `0.0.0.0`
   - `192.168.2.2` may also work
- Client endpoints for external use are configured to the external IP
   - e.g. `192.168.2.1` for connecting to a UDP server on the [Control Station Computer](@/integrations/hardware/required/control-computer/index.md)
- Client endpoints seem to operate more stably than server ones
- Unprotected endpoints can be removed or disabled
- New endpoints can be created, or existing unprotected ones can be modified
   - e.g. some users may wish to set up a UDP endpoint for connecting to with
   Pymavlink from the surface:
{% end %}
{{ easy_image(src="mavlink-endpoints-pymavlink-example", width=400, class="pirate", center=true) }}

{% pirate() %}
### MAVLink Inspector
{% end %}
{{ service(service="MAVLink2Rest", port=6040, link="https://github.com/mavlink/mavlink2rest") }}
{% pirate() %}

The MAVLink Inspector provides real-time access to the MAVLink messages being
sent to the topside computer. It is possible to
- filter for particular messages
- view past and current messages
- click on messages to see their full details
{% end %}
{{ easy_image(src="mavlink-inspector", width=600, class="pirate") }}
{% pirate() %}
Future improvements will include plotting and comparisons, along with more
powerful filtering options.

For tracking the latest value of a single message type, use the "watcher"
functionality of the `MAVLink2REST` service (access via the
[Available Services](#available-services) page).
{% end %}

### Network Test
{{ service(service="Pardal", port=9120, link="/services/pardal", based=true) }}

The Local Network Test measures real-time latency between BlueOS and the surface
computer, and allows checking the upload and download speeds between them.

A plot is provided of each test, to help diagnose intermittent issues.

{{ easy_image(src="network-test-local", width=350) }}

The Internet Speed Test allows measuring the latency and upload and download
speeds between BlueOS and its internet connection (if one is available).

{{ easy_image(src="network-test-internet", width=350) }}

{% pirate() %}
### NMEA Injector
{% end %}
{{ service(service="NMEA Injector", port=2748, link="/services/nmea_injector", based=true) }}
{% pirate() %}

- Conveys GPS positions (from an NMEA device) to the vehicle via MAVLink messages
{% end %}
{{ easy_image(src="nmea-injector", width=600, class="pirate") }}
{% pirate() %}
- Setup requires a UDP or TCP socket for the NMEA device to connect to, and a MAVLink ID
for the component that will send the location data to the vehicle
{% end %}
{{ easy_image(src="nmea-example", width=400, class="pirate", center=true) }}

### Ping Sonar Devices
{{ service(service="Ping Service", port=9110, link="/services/ping", based=true) }}

`New in 1.1`

The Ping Sonar Devices page shows any detected
[sonars](@/integrations/hardware/additional/sonars/index.md) from the Ping family,
including [ethernet-configured Ping360s](https://bluerobotics.com/learn/changing-communications-interface-on-the-ping360/#wiring-connections--ethernet-configuration)
that are visible on the local network (e.g. via an
[Ethernet Switch](@/integrations/hardware/additional/ethernet-switch/index.md)).

- Allows configuring Ping Sonar distance estimates to send as MAVLink
[`DISTANCE_SENSOR`](https://mavlink.io/en/messages/common.html#DISTANCE_SENSOR)
messages to the autopilot, for viewing in the Control Station Software and
logging as part of the telemetry stream
- Provides a viewing utility for devices connected via USB/serial, to show
which port they are plugged into

{{ easy_image(src="ping-sonar-devices", width=600) }}

{% pirate() %}
### Serial Bridges
{% end %}
{{ service(service="Bridget", port=27353, link="/services/bridget", based=true) }}
{% pirate() %}

The Serial Bridges page allows creating high performance links between serial
devices that are connected to the onboard computer, to a UDP port.

Replaces the [Routing](https://www.ardusub.com/reference/companion/routing.html)
functionality from the old Companion Software.

For making connections to the autopilot, see [MAVLink Endpoints](#mavlink-endpoints).
{% end %}
{{ easy_image(src="serial-bridges", width=600, class="pirate") }}
{% pirate() %}
- **NOTE:** UDP-based systems do not guarantee packet delivery or sequential alignment
- Bridges to the [Control Station Computer](@/integrations/hardware/required/control-computer/index.md)
  will generally use the localhost IP `0.0.0.0`, which creates a UDP server that waits
  for a UDP client on the control computer to connect to it
   - other IP addesses create a UDP client on the onboard computer, which expects the
     serial device to initiate communication before the connected UDP server (on the
     control computer) can respond
   - **NOTE:** a client should communicate with the server at least once every 10 seconds
     to avoid being disconnected
- Bridges to internal programs can use the loopback IP `127.0.0.1`, which creates a
  local server
- Allows setting a custom UDP port for a UDP client bridge to listen to responses at
{% end %}
{{ easy_image(src="serial-bridges-example", width=400, class="pirate", center=true) }}

### System Information
{{ service(service="System Information", port=6030) }}

The System Information page provides useful information about the processes,
network configuration, and computer system BlueOS is running on. It can be
useful for troubleshooting, and finding if a particular program is using
excessive resources.
{{ easy_image(src="system-info-processes", width=600) }}
{{ easy_image(src="system-info-monitor", width=600) }}
{{ easy_image(src="system-info-network", width=600) }}
{{ easy_image(src="system-info-kernel", width=600, class="pirate") }}
{{ easy_image(src="system-info-firmware", width=600, class="pirate") }}
{% pirate() %}
Update buttons are provided if the device is not running the latest stable versions of
the Raspberry Pi firmware or USB controller.
{% end %}
{{ easy_image(src="system-info-about", width=600) }}

{% pirate() %}
### Terminal
{% end %}
{{ service(service="ttyd" link="https://tsl0922.github.io/ttyd/", port=8088) }}
{% pirate() %}

The Terminal provides
- A tmux session
   - Can split horizontally (`CTRL+b "`) and vertically (`CTRL+b %`), and use cursor
   to resize the panels
   - For more advanced tips, check out the
   [tmux cheat sheet website](https://tmuxcheatsheet.com/)
- Direct access into the core BlueOS docker container
- Ready access to the tmux sessions of the core services (`CTRL+b s`)
   - Useful for seeing logs as they update live
   - Can kill  (`CTRL+c`) services, and start them again (`↑ ⏎`)
- Access to the underlying device via the `red-pill` utility
   - Can return to the core container using the `exit` command, or pressing `CTRL+d`
   - Can list available docker images (including extensions) with `docker image list`
   - Can list active docker containers (including extensions) with `docker ps -a`
   - For BlueOS host computers that do not have the default user as "pi", a custom username
     can be specified using the `-u` argument (e.g. `red-pill -u myusername`) `(New in 1.2)`
{% end %}
{{ easy_image(src="terminal", width=600, class="pirate") }}

### Vehicle Setup
`New in 1.1`

The Vehicle Setup page provides an overview of the vehicle, including its sensors and
peripherals. The 3D model can be rotated, and can be panned by clicking and dragging
while holding SHIFT. The camera icon can be used to capture a screenshot of the current
view of the model, with a transparent background.

{{ easy_image(src="vehicle-setup-overview", width=600) }}

{% pirate() %}
It is possible to override the displayed 3D model by providing 
[an appropriate glTF file](#vehicle-model).

In future this page will also allow
- using custom highlighting logic for model components
- displaying device statuses from extensions
{% end %}

The PWM Outputs tab allows configuring the servo function mappings
(for motors, lights, camera tilt, etc), as well as manually testing the motors,
and an automated check to detect motors that spin backwards. Relevant motors can be
set to run on reversed control signals, so they spin in the expected direction.

Hovering over a motor or output function will attempt to highlight the related component
in the 3D model.

{{ easy_image(src="vehicle-setup-pwm-outputs", width=600) }}

Clicking on an output in the right side table allows changing its assigned function,
and configuring its PWM limits and trim value `(New in 1.4)`:

{{ easy_image(src="vehicle-setup-pwm-function", width=450) }}

The Configure tab provides configuration and calibration options for the vehicle sensors and peripherals,
including failsafes, and reverting parameters to their defaults.
<br>`New in 1.3`

{{ easy_image(src="vehicle-setup-configure-params", width=600) }}

{{ easy_image(src="vehicle-setup-configure-gyro", width=600) }}

{{ easy_image(src="vehicle-setup-configure-accel", width=600) }}

{{ easy_image(src="vehicle-setup-configure-compass", width=600) }}

{{ easy_image(src="vehicle-setup-configure-baro", width=600) }}

{{ easy_image(src="vehicle-setup-configure-lights", width=600) }}

{{ easy_image(src="vehicle-setup-configure-failsafes", width=600) }}

`New in 1.4`
{{ easy_image(src="vehicle-setup-configure-gimbal", width=600) }}

### Video Streams
{{ service(service="MAVLink Camera Manager", link="https://github.com/bluerobotics/mavlink-camera-manager/", port=6020) }}

- BlueOS automatically detects H264-encoded video streams
{% pirate() %}
- MJPG and YUYV encoded streams are also detected in pirate mode, but currently only work when configured as RTSP streams
- H265 encoded streams can be streamed, using RTSP or UDP265 `(New in 1.4)`
{% end %}

- The first time BlueOS starts up it will auto-configure any cameras that are
connected at that time, with UDP streams counting up from port `5600`
   - e.g. a second camera at first startup would be streamed to port `5601`
   - Auto-configuration also occurs if the settings are reset
      - applies to both [global settings resets](#blueos-settings) (via the sidebar) and
      - camera manager settings resets (via the settings icon in the bottom right)
- After the initial startup, settings are saved and persistent across reboots
   - Further changes require manually re-configuring streams
   - New streams need to be manually added
      - UDP stream endpoints should be set to `udp://<surface-IP>:<port>`
         - e.g. `udp://192.168.2.1:5602`
      - RTSP stream endpoints are auto-configured with appropriate values
   - One video input can have multiple output streams by clicking the blue `+`
   symbol during stream configuration
      - This only works for streams of the same endpoint type (e.g. it is not
      currently possible to mix UDP and RTSP output streams for the same input)

{{ simple_pirate_image(src="video-stream-example", width=400, center=true) }}

- By default the streams are also presented via MAVLink, so QGroundControl (>=v4.1.7)
can toggle between them without needing to know specific ports
{% pirate() %}
   - It is possible to specify a stream as "thermal", which allows it to be
   overlaid on another stream in some viewing applications
{% end %}

{{ easy_image(src="qgc_switch_streams", width=400, center=true) }}

- Camera settings (brightness, exposure, etc) that are exposed via UVC can be
configured with the "Configure" button

{{ easy_image(src="video-config-example", width=500, center=true) }}

- Camera settings are also exposed via the
[MAVLink camera protocol](https://mavlink.io/en/services/camera.html), so are
controllable in QGroundControl
- Switching streams in QGroundControl while recording stops the current recording
   - If you are regularly switching streams it may be worth doing a screen recording
   either instead of or as well as recording the base video
- QGroundControl does not yet support displaying multiple streams simultaneously
   - Additional streams can be processed/viewed/recorded by [the options discussed
   here](https://discuss.bluerobotics.com/t/how-to-stream-another-cameras-video/9573/3#receiving-the-stream-7)
       - Note that some playback applications (e.g. VLC)  treat odd-numbered ports
       as audio channels, so relevant video streams should only use even-numbered
       ports
       - UDP streams have the option to download an SDP file (or copy a URL to it),
       for easier video playback in applications like VLC `(New in 1.1)`
- Raspberry Pi cameras are supported `(New in 1.1)`
   - Detection requires turning on legacy camera support:
      1. turn on via the settings button in the buttom right corner
      2. reboot the onboard computer to enable
{% pirate() %}
- It is possible to use the "Redirect source" element to make an ethernet camera
available via the BlueOS camera manager, which allows QGroundControl to detect
it automatically (via MAVLink)
{% end %}

{{ simple_pirate_image(src="video", width=600) }}

## Extensions

### Extensions Manager
`New in 1.1`
{{ service(service="Kraken", port=9134, link="/services/kraken", based=true) }}

The Extensions Manager is in charge of fetching, installing, updating, and managing
[Extensions](@/development/extensions/index.md).

The Store tab shows the available extensions, with a default filter which excludes
the development example extensions.

{{ easy_image(src="extensions-store", width=600) }}

Clicking an extension card displays the developer information, default application settings
and permissions, a description / basic usage instructions, and a dropdown to select which
version of the extension to install (or uninstall):

{{ easy_image(src="extensions-store-example", width=400) }}

By default, the store searches the
[BlueOS Extensions Repository](https://docs.bluerobotics.com/BlueOS-Extensions-Repository/)
for available extensions, but it is also possible to specify your own external collections
of extensions:
{{ easy_image(src="extensions-store-manifests", width=350) }}

The Installed tab shows the resource usage of the installed extensions, and allows
configuring them, checking their logs, and restarting or disabling them:
{{ simple_pirate_image(src="extensions-installed", width=600) }}


The blue "+" button in the bottom right corner allows installing custom extensions as relevant.
{% pirate() %}
The "Edit" button on installed extension listings allows changing to alternative/development
versions by setting the docker tag.
{% end %}

{{ simple_pirate_image(src="extensions-installed-example", width=400) }}

## Interface Theme

### Theme Content
#### Company Logo

- square images work best

{{ easy_image(src="theme-logo", width=500, center=true) }}

#### Vehicle Icon

- square images work best
- consider getting an image of your 3D model from the [Vehicle Setup](#vehicle-setup) page

{{ easy_image(src="theme-vehicle", width=500, center=true) }}

#### Vehicle Name and mDNS Hostname

- the vehicle name makes it easier to determine which vehicle you are connected to
- changing the mDNS hostname changes the address you connect to for the browser interface
    - wired connection (ethernet tether / USB-OTG) -> [http://custom.local](http://custom.local)
        - ethernet can also be accessed through [http://custom-ethernet.local](http://custom-ethernet.local)
    - wifi connection -> [http://custom-wifi.local](http://custom-wifi.local)
    - BlueOS hotspot -> [http://custom-hotspot.local](http://custom-hotspot.local)
    - [http://blueos.local](http://blueos.local) will still be available for wired connections,
    as a fallback in case the custom name is forgotten
    - [http://blueos-avahi.local](http://blueos-avahi.local) is broadcast to all interfaces,
    for connecting when you're not sure which interface(s) are available
    - IP addresses are always available for
    [the interfaces they're configured for](#wired-network-management-ethernet-usb-otg), but
    they're less intuitive to remember than mDNS names
    - **NOTE:** mDNS is available on most modern operating systems, but
        - for Windows older than Windows 10, [install Bonjour](https://support.apple.com/kb/dl999)
        - for Linux run `avahi-discover` on the terminal to see if the avahi service is running
           - For reference: [Debian](https://wiki.debian.org/Avahi), [Arch](https://wiki.archlinux.org/title/avahi)

{{ easy_image(src="theme-name-mdns", width=400, center=true) }}

{% pirate() %}
#### Vehicle Model

The 3D model used in the [Vehicle Setup](#vehicle-setup) pages can be replaced with a
custom glTF file:
{% end %}

{{ easy_image(src="vehicle-setup-custom-model", width=600, class="pirate") }}

{% pirate() %}
There are a variety of ways to create such a file, but our standard process is to:
1. Export a model of your vehicle as an STL file
    - triangle meshes are bad for designing, but nice for rendering
1. Import it into [Blender](https://www.blender.org/)
1. Remove small parts (including cutting off the threads of screws)
    - This helps improve performance, with minimal effect on what can be seen
1. Merge/group pieces that are part of a single component
    - Select them and press `CTRL+J`
    - We'll typically rename the groups with meaningful component names as we go
1. Add materials, to "paint" the parts in different colours
    - `CTRL+L` can help to select connected faces
    - Transparent materials can be made by setting low alpha values, and setting the blend mode to "Alpha Blend"
1. For parts that should sometimes be highlighted in the Vehicle Setup page, start the names of their materials with their [servo function enum name](https://github.com/bluerobotics/BlueOS/blob/master/core/frontend/src/types/autopilot/parameter-sub-enums.ts#L100-L162)
    - Not case sensitive
    - E.g. thruster 1 should only use materials with names that start with `motor1`
1. Merge the parts, set the shading to smooth, and adjust the auto-smoothing of normals to a value that looks right for your components
1. Reduce the model triangles by adding a decimate modifier
    - We typically drag the Ratio slider down until the model looks bad, then bring it back up until it looks acceptable
1. Export as a glTF (`.glb`) file, so it can be read by the BlueOS model viewer
    - `Mesh / Apply Modifiers` and `Compression` should both be enabled in the export configuration
1. Save it into the relevant folder in the BlueOS [File Browser](#file-browser)
    - `userdata/modeloverrides/<vehicle-type>/<vehicle-frame>.glb`
    - e.g. for a BlueROV2 Heavy, `<vehicle-type>` would be `sub`, and `<vehicle-frame>` would be `VECTORED_6DOF`
    - to ignore vehicle type switching, you can instead save the file as `/userdata/modeloverrides/ALL.glb`
1. If you want, it's possible to add annotations (like we have for the motors, in the default model) via the glTF https://modelviewer.dev/editor/
    - "Add Hotspot" to create a new annotation
    - Copy the resulting "data surface" attribute into a json file like the following:
        ```json
        {
            "annotations": {
                "Motor1": {
                    "surface": "3 1 2510 2509 2511 0.356 0.124 0.521",
                    "text": "Motor 1"
                },
                "Motor2": {
                    "surface": "2 1 2642 2643 2674 0.638 0.230 0.133",
                    "text": "Motor 2"
                },
            }
        }
        ```
        - annotation names should match the [`SERVOn_FUNCTION`](https://ardupilot.org/sub/docs/parameters.html#servo1-function-servo-output-function) parameter value names
        - Save the file in the same place as and with the same base name as the model file, e.g. `VECTORED_6DOF.json`

### Theme Styling
It is possible to customise the styling of the BlueOS interface by adding a
`theme_style.css` file at `userdata/styles/` in the [File Browser](#file-browser).
The File Browser can also be used to modify the file, in which case the styles
are updated at the next page refresh after the file is saved. The save button is
in the top right corner.

[CSS](https://www.w3schools.com/html/html_css.asp) is commonly used for styling
HTML webpages, and has an extensive set of features available. For the purposes of
adjusting the BlueOS theme, the most important thing to understand is
[how to specify colors](https://www.w3schools.com/colors/default.asp). It can be
helpful to use tools like [colorhexa](https://www.colorhexa.com/5eabed) when
choosing a palette of colors, including for checking accessibility for various
color vision deficiencies.

For reference, here is an example with most of the main BlueOS colors changed,
together with the theme file that created it:

{% end %}

{{ easy_image(src="theme-style-main", width=600, class="pirate") }}

{% pirate() %}
```css
:root {
  --v-primary-base: #CAB1E5 !important;  /* sidebar highlights, submit buttons */
  --v-info-base: #BA55E5 !important;     /* info boxes (often same as primary base) */
  --v-warning-base: #EDD1E5 !important;  /* warnings and skip buttons */
  --v-error-base: #AC1D1C !important;    /* notifications, pirate icons, negative buttons */
  --v-anchor-base: #5A11ED !important;   /* hyperlinks */
}

/* light theme background, light to dark */
div.light-background {
  background-color: #BAFF1E !important;
  background-image: linear-gradient(160deg, #BAFF1E 0%, #5CA1E5 100%) !important;
}

/* dark theme background, light to dark */
div.dark-background {
  background-color: #5EABED !important;
  background-image: linear-gradient(160deg, #5EABED 0%, #BA55E5 100%) !important;
}

/* light theme header bar background, light to dark, translucent */
header.light-background-glass {
  background-color: #DEADBA55 !important; /* fallback if gradient not available */
  background-image: linear-gradient(160deg, #DEADBA88 0%, #5111CA88 100%) !important;
  backdrop-filter: blur(4.5px) !important;
  -webkit-backdrop-filter: blur(10px) !important;
}

/* dark theme header bar background, light to dark, translucent */
header.dark-background-glass {
  background-color: #5111CA55 !important; /* fallback if gradient not available */
  background-image: linear-gradient(160deg, #5111CA88 0%, #0B5E5588 100%) !important;
  backdrop-filter: blur(4.5px) !important;
  -webkit-backdrop-filter: blur(10px) !important;
}
```

The explanatory diagram colours are also configurable `(New in 1.4)`:
{% end %}

{{ easy_image(src="theme-style-diagrams", width=600, class="pirate") }}

{% pirate() %}
```css
:root {
    --v-water-base: #AC2317 !important;
    --v-negative-base: #891A10 !important;
    --v-attention-base: #ECC19B !important;
    --v-positive-base: #514A3B !important;
    --v-neutral-base: #A15447 !important;
    --v-detail-base: #370502 !important;
    --v-outline-base: #FDF4C9 !important;
}
```
{% end %}


<script type="text/javascript">
    function toggle_advanced() {
        if (document.querySelector('.pirate').style.display == 'none') {
            var show = '.pirate';
            var hide = '.simple';
            var buttonPrefix = "hide";
        } else {
            var show = '.simple';
            var hide = '.pirate';
            var buttonPrefix = "show";
        }
        [].forEach.call(document.querySelectorAll(show), function(el) {
            el.style.display = 'block';
        });
        [].forEach.call(document.querySelectorAll(hide), function(el) {
            el.style.display = 'none';
        });
        document.getElementById("pirate_toggle").textContent =
            buttonPrefix+" advanced functionality";
    }
</script>
