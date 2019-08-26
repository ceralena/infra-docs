# infra-docs

This repository holds documentation about my personal infrastructure.

For now, everything's in the README. I'll break it up or convert it into a
static site if it gets unwieldy.

## Why is it public?

* to discourage myself from relying on security through obscurity
* to encourage myself to keep it up-to-date
* to get more public stuff on my GitHub

## Networking

Networks:

| Subnet Name           | Location  | CIDR             |
| ----------------      | ----------| ---------------- |
| HomeShared            | Melbourne | `192.168.1.1/24` |
| HomeInfrastructure    | Melbourne | `192.168.2.1/24` |

Devices:

* Model: old ADSL junk in bridge mode
* Router: [UniFi EdgeRouter X](https://www.ui.com/edgemax/edgerouter-x/)
* Wireless AP: [UniFi AP AC Lite](https://www.ui.com/unifi/unifi-ap-ac-lite/) - 802.11ac dual radio AP

## Hosts

### glow

This is a little server in a metal enclosure.

* Board: [PC Engines apu2c4](https://pcengines.ch/apu2c4.htm)
* Memory: 4GB DRAM
* Storage: 256gb mSata SSD
* Network: HomeShared

It has no video output, so the initial setup is done over a null modem cable.

It's running Ubuntu 18.04 (Bionic) and [MicroK8s](https://microk8s.io/).

### rup

This is my primary laptop.

* Year: 2018
* Machine: X1 Carbon Gen 6
* CPU: Intel Core i7-8550U (8M cache, 1.8GHz, 4.0GHz max)
* Memory: 16GB LPDDR3 2133mhz
* Storage: 512GB PCIe SSD
* Display: 14.0" 2560x1440 IPS
* Battery: 3 cell 757Wh

The drive is partitioned 50-50 between Windows 10 Home and Arch Linux. Windows
boots by default.

### niue

This is a desktop I built for work and gaming.

* Year: 2017
* CPU: AMD Ryzen 5 1600 6 Core AM4 CPU
* Video: GeForce GTX 1060 6GG
* Memory: Corsair 16GB DDR4 3000MHz

The drive is partitioned 50-50 between Windows 10 Home and Arch Linux. Windows
boots by default.
