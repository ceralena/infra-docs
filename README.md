# infra-docs

This repository holds documentation about my personal infrastructure.

For now, everything's in the README. I'll break it up or convert it into a
static site if it gets unwieldy.

- [infra-docs](#infra-docs)
  - [Why is it public?](#why-is-it-public)
  - [Networking](#networking)
    - [Internet Access](#internet-access)
    - [Networks](#networks)
    - [Networking Devices](#networking-devices)
    - [Wireless Networks](#wireless-networks)
  - [Hosts](#hosts)
    - [tube](#tube)
    - [glow](#glow)
    - [rup / mopl](#rup--mopl)
    - [niue](#niue)
    - [ChromeCast-Ultra](#chromecast-ultra)
    - [glow](#glow-1)
    - [bep](#bep)
  - [Troubleshooting](#troubleshooting)
    - [USG (router) not giving out DHCP](#usg-router-not-giving-out-dhcp)

## Why is it public?

* to discourage myself from relying on security through obscurity
* to encourage myself to keep it up-to-date
* to get more public stuff on my GitHub

## Networking

### Internet Access

NBN FTTC 100/40 through Aussie Broadband as of 2019-11.

### Networks

| Subnet Name      | Domain          | Location  | CIDR              | VLAN ID |
| ---------------- | --------------- | --------- | ----------------- | ------- |
| CentralTrusted   | central-trusted | Melbourne | `192.168.1.0/24`  | n/a     |
| CentralIOT       | central-iot     | Melbourne | `192.168.69.1/24` | 69      |

By convention, the VLAN ID matches the third octet.

Soon, I'll add default-deny rules for traffic between subnets - with some exceptions like the Bastion host.

### Networking Devices

| Role           | Model                                                                                                             | Comment            |
| -------------- | ----------------------------------------------------------------------------------------------------------------- | ------------------ |
| Modem          | [NBN Fibre to the Curb NCD](https://www.nbnco.com.au/learn/network-technology/fibre-to-the-curb-explained-fttc)   |                    |
| Router         | [UniFi Security Gateway](https://www.ui.com/unifi-routing/usg/)                                                   | "USG"              |
| Switch         | [UniFi Switch 24 PoE+](https://www.ui.com/unifi-switching/unifi-switch-2448/)                                     | in central closet  |
| Switch         | [UniFi Switch 8](https://www.ui.com/unifi-switching/unifi-switch-8/)                                              | in living room     |
| Wireless AP    | [UniFi AP AC Lite](https://www.ui.com/unifi/unifi-ap-ac-lite/)                                                    | on hallway ceiling |
| SDN Controller | [UniFi Cloud Key](https://www.ui.com/unifi/unifi-cloud-key/)                                                      |                    |

The Switch isn't L3-native, so routing between VLANs happens on the USG.

### Wireless Networks

| SSID                | VLAN ID | Spectrum    | Role            |
| ------------------- | ------- | ----------- | --------------- |
| `whats-up-bb-hru`   | n/a     | 5GHz only   | trusted devices |
| `whats-up-bb-iot`   | 69      | 2.4GHz only | trusted devices |

## Hosts

### tube

**Network**: CentralTrusted

This is an ODROID-N2 running CoreElec as a Kodi media centre.

* Board: [ODROID-N2](https://www.hardkernel.com/shop/odroid-n2-with-4gbyte-ram/)
* Memory: 4GB RAM
* Storage: 6TB and 2TB USB3 mechanical drives

It supports 4k 60hz HDMI 2.0 output. It also has transmission running as a daemon.

### glow

**Network**: CentralBastion

This is a little server in a metal enclosure.

* Board: [PC Engines apu2c4](https://pcengines.ch/apu2c4.htm)
* Memory: 4GB DRAM
* Storage: 256gb mSata SSD

It has no video output, so the initial setup is done over a null modem cable.

It's running Ubuntu 18.04 (Bionic) and [MicroK8s](https://microk8s.io/).

It's used as a bastion host for the network.

### rup / mopl

**Network**: CentralTrusted

This is my primary laptop.

* Year: 2018
* Machine: X1 Carbon Gen 6
* CPU: Intel Core i7-8550U (8M cache, 1.8GHz, 4.0GHz max)
* Memory: 16GB LPDDR3 2133mhz
* Storage: 512GB PCIe SSD
* Display: 14.0" 2560x1440 IPS
* Battery: 3 cell 757Wh

The drive is partitioned 50-50 between Windows 10 Home and Arch Linux. Arch boots by default.

Hostname is `rup` under Windows, `mopl` under Arch.

### niue

**Network**: CentralTrusted

This is a desktop I built for work and gaming.

* Year: 2017
* CPU: AMD Ryzen 5 1600 6 Core AM4 CPU
* Video: GeForce GTX 1060 6GG
* Memory: Corsair 16GB DDR4 3000MHz

The drive is partitioned 50-50 between Windows 10 Home and Arch Linux. Windows
boots by default.

### ChromeCast-Ultra

**Network**: CentralTrusted

This is a Chromecast Ultra supporting 4k ouptut.

* Year: 2019

### glow

**Network**: CentralTrusted

This is an [ODROID-N2](https://www.hardkernel.com/shop/odroid-n2-with-4gbyte-ram/) single board computer plugged into my TV.

It runs CoreELEC, a fork of LibreElec. It handles 4k 60hz HDMI output smoothly and doesn't skip a beat decoding HEVC.

* Year: 2019
* CPU: Quad-core Cortex-A73 (1.8Ghz) and Dual-core Cortex-A53 (1.9Ghz); ARMv8-A with Neon and Crypto extenions
* Video: HDMI 2.0 with 4K@60Hz
* Memory: 4Gib DDR4
* Networking: GbE LAN port connected to a Unifi Switch 8

### bep

**Network**: CentralIOT

This is a [Raspberry Pi 3 Model B](https://www.raspberrypi.org/products/raspberry-pi-3-model-b/) running [Raspbian](https://en.wikipedia.org/wiki/Raspbian) Buster.

* Year: 2016
* CPU: Quad Core 1.2GHz Broadcom BCM2837
* Memory: 1GB RAM

This is the only trusted device on the `central-iot` network. It _will be_ used for monitoring and controlling IOT stuff, when I get around to it.

## Troubleshooting

### USG (router) not giving out DHCP

On Thursday 2020-03-05, I had an NBN outage due to flooding on the street; the fiber and the device in the pit had to be replaced.

After service was restored - around 13:30 on Saturday 2020-03-07 - my wifi SSID was not available- even after restarting the USG, the switch and the AP several times. I narrowed the issue down to the USG: it was not servicing DHCP, even with my laptop connected directly to the LAN1 port.

This was quite frustrating given my investment in this gear and the setup - but thankfully the fix was reatively simple.

I decided to factory reset it with the pin button on the front; thankfully, I'd left the controller on the same subnet as the default subnet the USG uses when it starts up, so I just needed to re-adopt it in order to roll out all the established config.

The process is:

1. power down the switch (which also powers the APs and the controller)
2. factory reset the USG
3. power up the switch
4. connect a laptop over wifi or ethernet
5. go to the web UI for the controller (`192.168.1.9`) - the `.localdomain` address for it doesn't work until the USG is fully configured
6. go to devices; "forget" the USG
7. re-adopt the USG
8. wait for all the config to roll out

Interestingly, even after my Chromecast Ultra reconnected, Android devices couldn't detect it, and the Google Home app said it was "not available". I had to restart my phone before apps that support Chromecast would show the cast button.