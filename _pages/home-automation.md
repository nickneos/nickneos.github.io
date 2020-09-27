---
title: Home Automation
header:
  image: /img/homeautomation_2x1.jpg
  # overlay_filter: 0.4
  caption: "Photo credit: [Bence Boros](https://unsplash.com/photos/anapPhJFRhM)"
classes: wide
permalink: /home-automation/
---



I have been using [Home Assistant](https://www.home-assistant.io) for several years to control and automate numerous devices within my home.

I am currently running [Home Assistant Core](https://www.home-assistant.io/faq/ha-vs-hassio/) within docker on a [HP Microserver Gen8](https://support.hpe.com/hpesc/public/docDisplay?docId=emr_na-c03793258) with [Debian 10](https://wiki.debian.org/DebianBuster).

Within my house I am controlling various smart devices via Home Assistant such as:
* [Philips Hue Lights](https://www.philips-hue.com)
* [Logitech Harmony Hub](https://www.logitech.com/en-au/product/harmony-hub)
* [Broadlink RM Wifi + IR + RF Remote Controller](http://www.ibroadlink.com/rm/)
* [Xiaomi Aqara Gateway](https://www.home-assistant.io/components/xiaomi_aqara/)
* [Tasmota Devices](https://tasmota.github.io/docs/)

My full configuration is [here](https://github.com/nickneos/Home-Assistant-Config).

## Home Assistant Posts

{% assign entries_layout = page.entries_layout | default: 'list' %}
<div class="entries-{{ entries_layout }}">

    {%- for post in site.posts -%}
        {% if post.tags contains 'home-assistant' %}
            {%- unless post.hidden -%}
                {% include archive-single.html type="grid" %}
            {%- endunless -%}
        {% endif %}
    {%- endfor -%}

</div>




 
