+++
title = "Installation"
description = "BlueOS installation instructions."
date = 2025-04-25T08:00:00+10:00
template = "docs/page.html"
sort_by = "weight"
weight = 10
draft = false
aliases = ['/software/onboard/BlueOS-latest/installation', '/blueos/latest/installation']

[extra]
toc = true
top = false
+++
## Download

BlueOS is a ground-up rewrite software to replace Companion. To use it you'll need to download and flash an SD card.
It is compatible with **Raspberry Pi 3**[^1], **4**, and **5**.

![Latest Stable](https://img.shields.io/github/v/release/bluerobotics/blueos.svg?label=Latest%20Stable)![Date](https://img.shields.io/github/release-date/bluerobotics/blueos?label=Date)

![Latest Beta](https://img.shields.io/github/v/tag/bluerobotics/blueos.svg?label=Latest%20Beta)![Date](https://img.shields.io/github/release-date-pre/bluerobotics/blueos?label=Date)

Recommended operating system images of the latest stable version can be downloaded here:

| Board Hardware | Image File (with Base OS) | Notes |
| --- | --- | --- |
| Raspberry Pi 4B | <a id="v7-bullseye">ARMv7 (32-bit) Bullseye</a>[^1] | Standard on Blue Robotics vehicles |
| Raspberry Pi 5 | <a id="v8-bookworm">ARMv8 (64-bit) Bookworm</a>[^2] | Limited testing |

Additional prebuilt operating system images are available in the [releases](https://github.com/bluerobotics/BlueOS/releases),
along with Docker images for ARMv7/ARMv8 and AMD64 platforms, as well as details of the main changes between different versions.

[^1]:ARMv7 image can also be used for Raspberry Pi 3B boards, but doing so is not recommended for new projects, and is expected to stop being supported in future BlueOS versions.

[^2]:ARMv8 image _may_ also work for the Raspberry Pi 4B, but is not actively tested or supported.



## Flash

We recommend using a fresh SD card with at least 32GB[^3] capacity, although more storage is recommended for onboard data recording (especially for high bandwidth data like video streams and imaging sonar).

1. Download and install [Balena Etcher](https://www.balena.io/etcher/)
1. Insert the SD card to your computer (you may need an SD card reader)
1. Open Etcher, select the image you just downloaded, and flash it onto the SD card

[^3]:SD cards with less than 8GB capacity are not expected to load BlueOS, and 16GB or below may suffer from reduced performance, particularly if also installing BlueOS Extensions.

## Run

1. Eject your SD card with the new BlueOS software
1. Insert it into your Raspberry Pi, and power it up!
   - The first boot may take a couple of minutes, as it expands the filesystem to the new SD card capacity
      - It should take around 2 minutes for a 16GB class 10 SD card
1. BlueOS is a _headless_ operating system, and uses a web interface rather than HDMI to a monitor
   - See the [Getting Started](../getting-started/) section for how to connect


## Updates

Once BlueOS is installed, updating to a different version is simple via the [Version Chooser](../advanced/#blueos-version).

## Manual Installation

For developers with alternative hardware, or who would rather install over a pre-installed base operating system / image, BlueOS provides an [install directory](https://github.com/bluerobotics/BlueOS/tree/master/install) with utilities to help perform manual/software-based installations.


<script type="text/javascript">
async function fetchLatestReleaseInfo() {
  const url = "https://api.github.com/repos/bluerobotics/BlueOS/releases/latest";

  const response = await fetch(url)
  if (!response.ok) {
    throw new Error(`Failed for fetch latest release info: ${response.statusText}`)
  }

  const info = await response.json()
  return info
}

function setLinkURL(aID, artifact) {
  try {
    document.getElementById(aID).setAttribute("href", artifact.browser_download_url);
  } catch (error) {
    console.error(`Failed to set ${aID} link: ${error.message}`)
  }
}

async function setDownloadURLs() {
  const images = ["v7-bullseye", "v7-bookworm", "v8-bookworm"];
  try {
    const releaseInfo = await fetchLatestReleaseInfo()
    releaseInfo["assets"].forEach((artifact) => {
      const name = artifact.name;
      if (name.endsWith(".zip")) {  // probably an RPi image
        images.forEach((elementID) => {
          if (name.includes(elementID)) {
            setLinkURL(elementID, artifact);
            console.log(`Set ${elementID} link to ${artifact.name} file download URL.`)
          }
        })
      }
    })
  } catch (error) {
    console.error(`Error: ${error.message}`)
  }
}

setDownloadURLs()
</script>
