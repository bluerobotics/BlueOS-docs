+++
title = "Data Privacy"
description = "BlueOS data collection and usage documentation."
date = 2024-10-15T05:30:00+11:00
template = "docs/page.html"
sort_by = "weight"
weight = 40
draft = false

[extra]
lead = ''
toc = true
top = false
+++

## Context

Any device which connects to the internet provides some information about itself and its user in doing so. How that information is processed, stored, and used determine whether it is a potential privacy concern.

As open source software, BlueOS can be freely independently reviewed and audited for privacy risks, and we encourage users to educate themselves on what data is exposed through connecting your vehicle to the internet and making use of the services within BlueOS.

## Intent

1. Anonymous usage data and statistics are collected to inform the development direction, identify problems within BlueOS and related software and hardware, and share insights with the community
1. No data is collected for or sold to advertisers

## Data Collection and Usage Details

| Service | Domain | Data | Usage |
| --- | --- | --- | --- |
| [Internet connectivity check](https://github.com/bluerobotics/BlueOS/blob/master/core/services/helper/main.py#L70) | telemetry.blueos.cloud | - IP address<br>- hardware identifier<br>- BlueOS version | - aggregate rough [distribution of recently active vehicles](https://blueos.cloud/), using [GeoIP](https://arxiv.org/pdf/2109.13665)<br>- estimating proportions of onboard computer types and flight controller boards<br>- estimating proportions of in-use BlueOS versions |
| Internet connectivity check | - firmware.ardupilot.org<br>- amazon.com<br>- github.com<br>- 1.1.1.1 (Cloudflare) | IP address | availability of autopilot, BlueOS, and Extension updates, and the internet speed test service |

## Privacy Protections

Anonymous usage data can provide valuable development insights and improvements with minimal risk or negative impact to individual users. That said, BlueOS does not require an internet connection for its basic operating features, so if you wish to avoid or obscure usage data being sent from your vehicle, you can:

1. Use a VPN service to mask your IP address, and present your vehicle as operating from somewhere else in the world
    - These services often cost money, and may slow down updates and Extension installations by reducing your network bandwidth
1. Set up rules in your firewall and/or router to block access to specific domains
    - This will prevent using related BlueOS services, although it is generally possible to use an offline workaround
1. Completely avoid connecting your BlueOS vehicle to the internet
    - This will prevent access to all online BlueOS services, so updates and installations would need to be performed manually or avoided
