+++
title = "Overview"
description = "BlueOS overview."
date = 2024-04-10T11:45:00+10:00
template = "docs/page.html"
sort_by = "weight"
weight = 0
draft = false
aliases = ['/software/onboard/BlueOS-latest/overview', '/blueos/latest/overview']

[extra]
lead = 'BlueOS is a modular, robust, and efficient platform for managing a vehicle or robot from its onboard computer.'
toc = true
top = false
+++

{{ easy_image(src="blueos-banner", width=460, center="true") }}
{{ easy_image(src="interface-highlight", width=650, center="true") }}

## A bit of context...

The [original Companion Software](https://www.ardusub.com/reference/companion-web-ui.html) project (started in 2015) was originally created with the simple intent to route an underwater vehicle's video stream and communications to the surface computer, and provide some basic configuration of those features and the vehicle firmware. The simple scope was great to get things started, but also meant that new and complex features weren't designed in from the start, so maintenance and developing functionality became increasingly challenging.

With lessons learned on useful features and software architecture requirements, BlueOS was designed and created from the ground up to fit the requirements of the onboard computer system we _want_ to have - with room to grow into a true operating system for the vehicle. BlueOS is modular to the heart, which makes it portable, robust to update, and extensible. 

We're super excited about our future with BlueOS, and we can't wait for you to join us and try it out! 😄

## What's in a BlueOS vehicle?

BlueOS is designed to integrate nicely with connected parts of the hardware and software stack involved in running and operating a vehicle.

In general, BlueOS (and its extensions) run on an [onboard computer](@/integrations/hardware/required/onboard-computer/index.md), and help an underlying autopilot (on a [flight controller board](@/integrations/hardware/required/flight-controller/index.md)) to communicate with an operator (via a [control station computer](@/integrations/hardware/required/control-computer/index.md)), while also interfacing with and passing on data from sensors that do not provide direct measurements of the vehicle's control state.

Here is a visual overview of common components in a BlueOS-related software stack:
{{ easy_image(src="stack", width=600, center="true") }}

## Project principles and goals

As the core development team we've tried to envision the future of the onboard computer, and the features that will require. Our initial ideas have been distilled into the following core concepts, many of which are already built in to the BlueOS of today:

* An interface that is **simple by default but powerful when needed** - the user has the power to change anything they desire and customize the full experience
* **Designed to focus on what matters**, improving user access to information and controls with a human-friendly UI and UX
* **Make complex tasks simpler** and improve ease of use by reusing design patterns from other applications (based on the [material UI guidelines](https://material.io/design/guidelines-overview))
* **Advanced error handling and detection**, making any problems clear to the user and developers, along with how to fix them
* **Simplify development**, providing full access to our services API and modular development model
* **Encourage contributions**, [the project is open source](https://github.com/bluerobotics/BlueOS)!
* **Portable and flexible**, you should be able to run on a Raspberry Pi 3/4 or any SBC with Linux operating system, contributions are welcomed
* **Highly functional with low CPU usage**, the entire system is built to run efficiently
* **Developed on solid foundations**, critical parts or intensive workforce services are designed using the most advanced languages and features available for stability

Some of these principles will only be evident in future releases, but the underlying software architecture and organization have been designed from the ground up to support and enable them.

## What's New in BlueOS-1.4?

This covers a summary of the major changes and new features in BlueOS-1.4. Where applicable relevant features are also included in the [feature comparison table](#feature-comparison). For detailed coverage of every change, please see the [full release notes](https://github.com/bluerobotics/BlueOS-docker/releases).

### Safety
- Added [Safe Mode](../advanced/#safe-mode), to avoid accidentally performing unsafe configuration while the vehicle is armed

### [Extensions](@/development/extensions/index.md)
- Improved [`register_service` specification](@/development/extensions/index.md#web-interface-http-server) to better support cross-communication between extensions, and remote access of extension interfaces

### Header Bar
- Added [network usage widget](../advanced/#header-indicators-and-blueos-configuration)
- Added [second GPS status indicator, and GPS yaw support](../advanced/#system-status)

### Page improvements
- [Autopilot Parameters](../advanced/#autopilot-parameters)
   - Added parameter description override functionality, for custom parameter support
- [MAVLink Endpoints](../advanced/#mavlink-endpoints)
   - Added MAVLink Server as a routing alternative, with all-endpoint logging and a detailed debugging interface
- [Vehicle Setup](../advanced/#vehicle-setup)
   - Added camera mount configuration options
   - Added GPS yaw to the compass page (when available)
- [Video Streams](../advanced/#video-streams)
   - Added support for H265-encoded video streams

### Device/Hardware Support
- Added support for running the Navigator [flight controller](@/integrations/hardware/required/flight-controller/index.md) with 64-bit operating systems

### [Data Privacy](../privacy/)
- Added Sentry reports for user-generated feedback

## Feature Comparison

BlueOS has almost all features from the old Companion, and several hotly-requested new ones too!

{% horizontal_scroll(width="1000px") %}
| Feature | BlueOS 1.3 | BlueOS 1.2 | BlueOS 1.1 | BlueOS 1.0 | Companion |
|---|---|---|---|---|---|
| [**Onboard Computer**](@/integrations/hardware/required/onboard-computer/index.md) | &rarr;<br>+ Raspberry Pi CM4 | &rarr; | &rarr;<br>+ Other Linux-based SBCs images to come | + Raspberry Pi 3B / 3B+ / 4B supported<br>+ You can install from scratch using the installation script in any Linux computer. (Modifications may be necessary for your hardware configuration) | Raspberry Pi 3B required |
| [**Flight Controller**](@/integrations/hardware/required/flight-controller/index.md) | &rarr; | &rarr; | &rarr;<br>+ Cube Orange<br>+ Pixhawk 6X | &rarr;<br>+ Navigator<br>+ Pixhawk 4 | Pixhawk |
| [**Video Streams**](../advanced/#video-streams) | &rarr;<br>+ RTSP variants | &rarr; | &rarr;<br>+ MPEG and YUYV encodings<br><br>+ Supports Raspberry Pi cameras | + Easily manage *multiple streams*<br><br>+ UDP and RTSP outputs<br><br>- Audio streaming<br>*not yet supported* ([#990](https://github.com/bluerobotics/BlueOS-docker/issues/990)) | Select a *single* camera to stream over UDP<br>+ Supports Raspberry Pi cameras ([except HQ Camera](https://discuss.bluerobotics.com/t/raspberry-pi-camera-stream-not-working/11976/18))<br>+ Supports a single audio stream over UDP |
| [**WIFI Manager**](../advanced/#indicators-and-network-configuration) | &rarr;<br>+ External adapter support | &rarr; | &rarr;<br>+ Vehicle provides local hotspot | &rarr;<br>+ Connect to and manage *multiple networks*, like a cellphone or computer WIFI manager | Connect to a *single network*<br>+ Visible and hidden networks supported |
| [**Ethernet Manager**](../advanced/#indicators-and-network-configuration) | &rarr; | &rarr; | &rarr; | *Multiple* static IPs *and* DHCP configuration | *Single* DHCP (client or server) *or* static network |
| [**Notification system**](../advanced/#header-indicators-and-blueos-configuration) | &rarr; | &rarr; | &rarr; | Notifications about issues, new releases, and the status of your system. | - |
| [**File Browser**](../advanced/#file-browser) | &rarr; | &rarr;<br>+ Folder for extension data and configuration files | &rarr; | &rarr;<br>+ *Edit files* from the browser | Download and upload files |
| [**Log Browser**](../advanced/#log-browser) | &rarr; | &rarr; | &rarr; | *Download and manage logs* from the browser<br>+ *Visualise and analyse logs* from the built in viewer | Ssh/terminal only |
| [**MAVLink inspector**](../advanced/#mavlink-inspector) | &rarr; | &rarr; | &rarr;<br>+ MAVLink2REST "watcher" option for individual message types | See and *inspect MAVLink messages in real time* from the browser | See latest MAVLink messages via MAVLink2REST |
| [**Network test**](../advanced/#network-test) | &rarr; | &rarr; | &rarr;<br>+ Graph during speed tests | &rarr;<br>+ Check *real time latency* | Check upload and download speed from the Control Station Computer to the vehicle's Onboard Computer |
| [**System information**](../advanced/#system-information) | &rarr; | &rarr; | &rarr; | Provides all the necessary information about the hardware, operating system, running processes, CPU, memory, disk, network usage and status | Basic usage statistics, list of connected devices |
| [**Web Terminal**](../advanced/#terminal) | &rarr; | &rarr;<br>+ Support for non-`pi` users | &rarr; | &rarr;<br>+ Uses a tmux session | Access Linux terminal from the browser |
| [**Autopilot Firmware**](../advanced/#autopilot-firmware) | &rarr; | &rarr; | &rarr; | &rarr;<br>+ *General ArduPilot* downloads;<br>+ *select vehicle* to update | `stable`, `beta`, and `devel` releases, custom uploads, and restore default parameters;<br>*ArduSub-only* downloads |
| [**Autopilot Parameters**](../advanced/#autopilot-parameters) | &rarr;<br>+ Intuitive ArduPilot calibrations and configuration<br>+ PX4 parameter descriptions | &rarr; | View, search, and edit ArduPilot vehicle parameters | - | - |
| [**Version Chooser**](../advanced/#blueos-version) | &rarr; | &rarr; | &rarr;<br>+ Bootstrap updates | + *Easily update/downgrade* between BlueOS versions, including locally stored<br>+ Includes *stable, beta, and master* releases*<br>+ Available even if main site failing | Update Companion to *latest stable only* |
| [**MAVLink Endpoints**](../advanced/#mavlink-endpoints) | &rarr;<br>+ Log messages in `.tlog` files | &rarr;<br>+ Choose routing service | &rarr; | &rarr; | Create and manage UDP, TCP, and serial MAVLink endpoints |
| [**NMEA support**](../advanced/#nmea-injector) | &rarr; | &rarr; | &rarr; | &rarr; | Conveys GPS positions to the vehicle |
| [**Ping Sonar Devices**](../advanced/#ping-sonar-devices) | &rarr; | &rarr; | &rarr;<br>+ Detects Ping360 in ethernet configuration<br>+ Ping Sonar distance estimates can be *sent via MAVLink* | &rarr;<br>+ Devices can be *hot-plugged*<br><br>- *No MAVLink pipeline* | Ping Sonar and Ping360 can connect with [Ping Viewer](https://docs.bluerobotics.com/ping-viewer/)<br>+ Ping Sonar distance estimates can be *sent via MAVLink* |
| [**Serial Bridges**](../advanced/#serial-bridges) | &rarr;<br>+ Separate target and listener ports | &rarr; | &rarr; | &rarr; | Create and manage bridges between serial and UDP/TCP endpoints |
| **Water Linked** | &rarr; | &rarr; | DVL-A50 and UGPS extensions available through Extensions Manager | [DVL-A50 package available](https://discuss.bluerobotics.com/t/external-integrations-extensions/10912#integration-example-dvl-5) | Supports UGPS and DVL-A50 |
| [**Extensions**](../extensions/) | &rarr; | &rarr; | Custom extensions available through Extensions Manager | &rarr; | Custom functionality requires forking the codebase |
{% end %}

## Release Types
BlueOS has multiple release types, to allow choosing your preferred balance between access to the latest fixes and improvements, and stability of the software. The three release types are:
- **Stable:** Officially tested and validated
   - Stable versions with long term support
   - Recommended for most users
- **Beta:** Quick-passed rolling releases with new features, bug fixes and general improvements
   - Versions that will be released after an internal test
   - A taste of what's to come
- **Master:** Rapidly-passed *bleeding edge* development releases 🔥
   - The very latest features, that may not have been tested yet
   - Highly volatile, generally not recommended
   - For those who want to live in the future

When BlueOS is connected to the internet, a notification appears if a newer version of the same release or a *stabler* type is available. E.g: When using a **Stable** version, _**only**_ new **Stable** versions will trigger a notification. If using a **Beta** release, newer **Beta** and **Stable** releases will trigger a notification. When running **Master**, any release type newer than the active one will trigger a new update notification. This helps to ensure that any updates will be as or more stable than your current version, unless you intentionally change to a less stable release type.

It's worth noting that the [Version Chooser](../advanced/#blueos-version) in general offers several major robustness and versatility improvements over the previous 'latest update only' approach, which should benefit both users and developers.

## Quick links

1. [Documentation](@/_index.md)
2. [Source code](https://github.com/bluerobotics/BlueOS)
3. [Releases, changelogs, files](https://github.com/bluerobotics/BlueOS/releases)
