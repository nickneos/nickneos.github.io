---
title: Sonoff Zigbee Bridge
tagline: Using a Sonoff Zigbee Bridge with the ZHA Integration on Home Assistant
categories:
  - Home-Assistant
tags:
  - home-assistant 
  - zha
  - zigbee
  - tasmota
  - sonoff
---

The <a target='_blank' href='https://www.banggood.com/custlink/3mKyWf8e9I' title=''>SONOFF Zigbee Bridge</a> is a relatively new device from Sonoff that can be used with <a target='_blank' href='https://s.click.aliexpress.com/e/_dW4XGDB'>Sonoff Zigbee Sensors</a>. However the full potential of this device is unlocked when you flash it with [Tasmota](https://github.com/arendst/Tasmota) firmware and add it to your [Home Assistant](https://www.home-assistant.io/) setup via the [ZHA Integration](https://www.home-assistant.io/integrations/zha/). This allows you to use the Sonoff Zigbee Bridge with just about any zigbee device from any manufacturer. Full compatibility [here](https://zigbee.blakadder.com/index.html).

![Sonoff Zigbee Bridge](/assets/images/blog/sonoff-zigbee-bridge.jpg){: .align-center}

In my house, I have numerous <a target='_blank' href='https://www.amazon.com/gp/search/ref=as_li_qf_sp_sr_tl?ie=UTF8&tag=nickneos-20&keywords=philips hue&index=aps&camp=1789&creative=9325&linkCode=ur2&linkId=8a5fc133915fff812219684bd6a0591b'>Philips Hue</a> Lights and dimmer switches controlled via the Philips Hue Bridge, as well as numerous <a target='_blank' href='https://s.click.aliexpress.com/e/_dUS36Y9'>Xiaomi Aqara devices</a> (temperature sensors, door sensors, wireless switches, etc) controlled via the Aqara Gateway. 

I've always wanted to be able to get rid of both the Philips Bridge and Aqara Gateway and use just one zigbee co-ordinator. While there were a few different devices already on the market to do this, I finally decided to make the move when the Sonoff released their Zigbee Bridge as I have been using Sonoff devices for a while with Tasmota firmware.

Flashing the Sonoff Zigbee Bridge with Tasmota so it can be used with Home Assistant directly via ZHA, will require you to open the device to access the PCB (printed circuit board). However no soldering is required so it is a relatively simple process. I will walk through the steps here. 


## Items Needed

First of all we need the **Sonoff Zigbee Bridge** itself. 

<a target='_blank' href='https://www.banggood.com/custlink/3mKyWf8e9I'><img src='https://imgaz.staticbg.com/images/oaupload/banggood/images/5C/E5/b686e369-9d49-474e-8c39-a60f869ef260.jpg' alt='' width='300'></a>

<sup>Purchase from: <a target='_blank' href='https://www.banggood.com/custlink/3mKyWf8e9I' title='' target="_blank">[BangGood]</a> <a href='https://s.click.aliexpress.com/e/_dTLT5hf' target="_blank">[AliExpress]</a> <a href="https://amzn.to/2EXprRj" target="_blank">[Amazon]</a></sup>


To flash the Zigbee Bridge, we need a **USB to TTL serial adapter**. 

<a target='_blank' href='https://www.banggood.com/custlink/KvDhr18Bia'><img src='https://imgaz.staticbg.com/images/oaupload/banggood/images/4A/1F/72fb76d9-d3ad-45a4-9323-e9924a3d7805.jpg' alt='' width='200'></a>

<sup>Purchase from: <a target='_blank' href='https://www.banggood.com/custlink/KvDhr18Bia' title=''>[BangGood]</a> <a href='https://s.click.aliexpress.com/e/_d7XaAi5' target="_blank">[AliExpress]</a> <a href='https://amzn.to/30vAVmA' target="_blank">[Amazon]</a></sup>

Also some jumper cables. 

<a target='_blank' href='https://www.banggood.com/custlink/DDDEpuI4oG'><img src='https://imgaz.staticbg.com/images/upload/2012/lidanpo/SKU099624h.jpg' alt='' width='200'></a>
<a target='_blank' href='https://www.banggood.com/custlink/vDmRpTI6lo'><img src='https://imgaz.staticbg.com/images/upload/2012/jiangjunchao/SKU067911h.JPG' alt='' width='200'></a>
<a target='_blank' href='https://www.banggood.com/custlink/KKvErwZBOi'><img src='https://imgaz.staticbg.com/images/2014/xiemeijuan/04/SKU197101/20130329152010137.JPG' alt='' width='200'></a>

**If you don't want to solder**, get male to male breadboard wires as these will fit into the vias (holes) on the PCB of the Zigbee Bridge. But you will also need female to female jumper wires to connect to the USB adapter.
  * <a target='_blank' href='https://www.banggood.com/custlink/DDDEpuI4oG' title='' > Male To Male Breadboard Wires</a>
  * <a target='_blank' href='https://www.banggood.com/custlink/vDmRpTI6lo' title='' >Female to Female Jumper Cable Dupont Wire</a>

If you're comfortable soldering, just get these if you don't have any lying around.
  * <a target='_blank' href='https://www.banggood.com/custlink/KKvErwZBOi' title='' >Male To Female Jumper Cable Dupont Wire</a>

Download the following:

* Flashing tool: [Tasmotizer](https://github.com/tasmota/tasmotizer/releases) 
* Tasmota Firmware: [tasmota-zbbridge.bin](http://ota.tasmota.com/tasmota/release/tasmota-zbbridge.bin) *(only needed if not flashing with Tasmotizer)*
* ZigBee Module Firmware: [ncp-uart-sw_6.7.6_115200.ota](https://github.com/arendst/Tasmota/raw/development/tools/fw_zbbridge/ncp-uart-sw_6.7.6_115200.ota) is the current stable (as of 2020-10-04) from [here](https://github.com/arendst/Tasmota/tree/development/tools/fw_zbbridge)

Also you will need all the devices you plan to pair to the Zigbee Bridge. Here are some that I use.

* <a target='_blank' href='https://www.amazon.com/gp/search/ref=as_li_qf_sp_sr_tl?ie=UTF8&tag=nickneos-20&keywords=philips hue&index=aps&camp=1789&creative=9325&linkCode=ur2&linkId=8a5fc133915fff812219684bd6a0591b'>Philips Hue Lights</a>
*  <a target='_blank' href='https://www.amazon.com/gp/search/ref=as_li_qf_sp_sr_tl?ie=UTF8&tag=nickneos-20&keywords=hue dimmer switch&index=aps&camp=1789&creative=9325&linkCode=ur2&linkId=45f73f96b34b4369965b073c25700135'>Philips Hue Dimmer Switch</a>
* <a target='_blank' href='https://s.click.aliexpress.com/e/_dUS36Y9'>Xiaomi Aqara Sensors</a>
* <a target='_blank' href='https://s.click.aliexpress.com/e/_dW4XGDB'>Sonoff Zigbee Sensors</a> 

## Flashing the Sonoff Zigbee Bridge

Remove the 4 rubber feet and unscrew the case to access the PCB.

If your USB serial adapter supports 5V and 3V3, **make sure the jumper or switch is set to 3V3!!!**

Connect your wires from the Zigbee Bridge to the USB adapter as follows:

| ZbBridge | USB Adapter |
| -------- | ----------- |
| 3v3      | 3V3/VCC     |
| GND      | GND         |
| ETX      | RX          |
| ERX      | TX          |
| IO0      | GND         |

![Sonoff Zigbee Bridge Wires](/assets/images/blog/sonoff-zigbee-wires.jpg){: .align-center}

If your USB adapter only has 1 ground connection, you can short IO0 and GND on the Zigbee Bridge.

Plug in the other end of the USB adapter to your computer and open up tasmotizer. Select the com port of your usb serial adapter, and the rest of the settings as shown below.

![Tasmotizer](/assets/images/blog/tasmotizer.jpg){: .align-center}

Click on Tazmotize and once its done flashing, disconnect the Zigbee Bridge (you can assemble it back in its case) and power it up.

On a PC or smartphone, check available WiFi Access Points and connect to the Tasmota one. 

![Tasmota Access Point](/assets/images/blog/tasmota-wifi.jpg){: .align-center}

Go to [http://192.168.4.1](http://192.168.4.1) in a browser. It should bring up a page like this:

![Tasmota WiFi Settings](/assets/images/blog/tasmota-zb1.jpg){: .align-center}

Fill out AP1 SSID and AP1 Password with your WiFi settings and then click save. The device will reboot.

Use an app like [Fing](https://play.google.com/store/apps/details?id=com.overlook.android.fing) to find out the IP address of your Zigbee Bridge. Use this IP address in a web browser, which should take you to the device's tasmota main menu:

![Tasmota WiFi Settings](/assets/images/blog/tasmota-zb2.jpg){: .align-center}

Now we need to flash the Zigbee module firmware. Click on *firmware upgrade* and then click on *choose file*. Select the file [ncp-uart-sw_6.7.6_115200.ota](https://github.com/arendst/Tasmota/raw/development/tools/fw_zbbridge/ncp-uart-sw_6.7.6_115200.ota) that you downloaded, and the click on *start upgrade*.

Give it a few minutes to flash, and when it's done it will reboot.

From the main menu, click on *console* and input the following to configure it to work with Home Assistant:
```sh
backlog rule1 on system#boot do TCPStart 8888 endon; rule1 1; template {"NAME":"SONOFF ZbBridge","GPIO":[56,208,0,209,59,58,0,0,0,0,0,0,17],"FLAG":0,"BASE":18}; module 0; devicename SONOFF ZbBridge; friendlyname SONOFF ZbBridge; topic ZbBridge
```

If you want the led light on the Zigbee Bridge to remain on when the device has power, enter this into the console:
```sh
ledpower 1
```

## Configuring Home Assistant

In Home Assistant, go to *configuration* > *integrations* and click the + button. Select *Zigbee Home Automation*.

![ZHA](/assets/images/blog/zha1.jpg){: .align-center}

Select *Enter Manually* for serial device path:

![ZHA](/assets/images/blog/zha2.jpg){: .align-center}

Select the *EZSP* option as the radio type:

![ZHA](/assets/images/blog/zha3.jpg){: .align-center}

For device path input `socket://<IP_ADDRESS>:8888` (replace `IP_ADDRESS` with the IP address of your ZigBee Bridge). Port speed is 115200.

![ZHA](/assets/images/blog/zha4.jpg){: .align-center}

Click finish.

![ZHA](/assets/images/blog/zha5.jpg){: .align-center}

You should now see a *Zigbee Home Automation* card in Integrations.

## Adding devices to the Zigbee Bridge

With our Zigbee Bridge setup via ZHA, we can start adding our varios zigbee sensors, lights, etc.

Click on *configure* on the *Zigbee Home Automation* card in Integrations.

![ZHA](/assets/images/blog/zha6.jpg){: .align-center}

Click the plus button.

![ZHA](/assets/images/blog/zha7.jpg){: .align-center}

Click on *show logs* and then put your device into pairing mode. If you're unsure how to put your device into pairing mode, refer [here](https://zigbee.blakadder.com/).

![ZHA](/assets/images/blog/zha8.jpg){: .align-center}

A card for the device will come up once its successfully paired with the device.

![ZHA](/assets/images/blog/zha9.jpg){: .align-center}

If you are finding it is not pairing, keep trying as they will eventually pair. I found that my philips hue lights connected almost straight away, but my aqara sensors took several attempts to connect. There are some good tips [here](https://community.hubitat.com/t/xiaomi-aqara-zigbee-device-drivers-possibly-may-no-longer-be-maintained/631) for pairing aqara devices.