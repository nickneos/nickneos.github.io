---
title: Sonoff Zigbee Bridge
subtitle: Using a Sonoff Zigbee Bridge with Home Assistant via ZHA Integration
description: Using a Sonoff Zigbee Bridge with Home Assistant via ZHA Integration
date: 2020-09-27
categories:
  - Home-Assistant
tags:
  - home-assistant 
  - zha
  - zigbee
  - tasmota
  - sonoff
published: true
canonical_url: https://www.nickneos.com/2020/09/27/sonoff-zigbee-bridge/
---

The <a target='_blank' href='https://www.banggood.com/SONOFF-ZBBridge-Smart-Bridge-Zigbee3_0-APP-Wireless-Remote-Controller-Smart-Home-Bridge-Works-With-Alexa-Google-Home-p-1674754.html?p=0Y26188200826201612A&custlinkid=1294644' title='' >SONOFF Zigbee Bridge</a> is a relatively new device from Sonoff that can be used with Sonoff Zigbee devices to control various sensors, lights etc. However the full potential of this device is unlocked when you flash it with [Tasmota](https://github.com/arendst/Tasmota) firmware and add it to your [Home Assistant](https://www.home-assistant.io/) setup via [ZHA](https://www.home-assistant.io/integrations/zha/). This allows you to use the Sonoff Zigbee Bridge with just about any zigbee device from any manufacturer. Full compatibility [here](https://zigbee.blakadder.com/index.html).

![Sonoff Zigbee Bridge](/assets/images/blog/sonoff-zigbee-bridge.jpg){: .align-center}

## My Use Case

In my home, I have numerous Philips Hue Lights and dimmer switches controlled via the Philips Hue Bridge, as well as numerous Xiaomi Aqara devices (temperature sensors, door sensors, wireless switches, etc) controlled via the Aqara Gateway. 

I've always wanted to be able to get rid of both the Philips Bridge and Aqara Gateway and use just one zigbee co-ordinator. While there were a few different devices already on the market to do this, 
I finally decided to make the move when the Sonoff released their Zigbee Bridge as I have been using Sonoff devices for a while with Tasmota firmware. Also the abilitiy to add zigbee devices from other manufacturers in future (IKEA, sonoff, etc) is also appealing.


## Items Needed

First of all we need the Sonoff Zigbee Bridge itself:

* <a target='_blank' href='https://www.banggood.com/SONOFF-ZBBridge-Smart-Bridge-Zigbee3_0-APP-Wireless-Remote-Controller-Smart-Home-Bridge-Works-With-Alexa-Google-Home-p-1674754.html?p=0Y26188200826201612A&custlinkid=1294644' title='' >SONOFF ZBBridge (purchase from BangGood)</a>

To flash the Zigbee Bridge, we also need a USB adapter:

* <a target='_blank' href='https://www.banggood.com/Geekcreit-FT232RL-FTDI-USB-To-TTL-Serial-Converter-Adapter-Module-Geekcreit-for-Arduino-products-that-work-with-official-Arduino-boards-p-917226.html?p=0Y26188200826201612A&custlinkid=1294645' title='' >FTDI USB To TTL Serial Converter Adapter</a>

Also some jumper wires. **If you don't want to solder**, get Male to Male breadboard wires, connected to Female to Female dupont jumper wires (since the breadboard wires will fit into the vias (holes) of the Zigbee Bridge's PCB). Otherwise you can just use Male to Female dupont jumper wires, but will need to solder as they will be too big for the vias on the PCB:

* If you don't want to solder
  * <a target='_blank' href='https://www.banggood.com/65pcs-Male-To-Male-Breadboard-Wires-Jumper-Cable-Dupont-Wire-Bread-Board-Wires-p-91799.html?p=0Y26188200826201612A&custlinkid=1294650' title='' > Male To Male Breadboard Wires</a>
  * <a target='_blank' href='https://www.banggood.com/40pcs-20cm-Female-to-Female-Jumper-Cable-Dupont-Wire-p-75612.html?p=0Y26188200826201612A&custlinkid=1294655' title='' >Female to Female Jumper Cable Dupont Wire</a>
* If you're comfortable soldering
  * <a target='_blank' href='https://www.banggood.com/40pcs-20cm-Male-To-Female-Jumper-Cable-Dupont-Wire-p-973822.html?p=0Y26188200826201612A&custlinkid=1294654' title='' >Male To Female Jumper Cable Dupont Wire</a>

Download the following:

* [Tasmotizer](https://github.com/tasmota/tasmotizer/releases) 
* [tasmota-zbbridge.bin](http://thehackbox.org/tasmota/release/tasmota-zbbridge.bin)
* [ncp-uart-sw_6.7.6_115200.ota](https://github.com/arendst/Tasmota/tree/development/tools/*fw_zbbridge) 

## Flashing the Sonoff Zigbee Bridge with Tasmota

Remove the 4 rubber feet and unscrew the case to access the PCB.

If your FTDI usb adapter supports 5V and 3V3, **make sure the jumper is set to 3V3!!!**

Connect your wires from the Zigbee Bridge to the FTDI adapter as follows:

| ZbBridge | FTDI Adapter |
| -------- | ------------ |
| ETX      | RX           |
| ERX      | TX           |
| IO0      | GND          |
| GND      | GND          |
| 3v3      | 3V3/VCC      |

![Sonoff Zigbee Bridge Wires](/assets/images/blog/sonoff-zigbee-wires.jpg){: .align-center}

