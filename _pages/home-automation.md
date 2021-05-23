---
title: Home Automation
header:
  overlay_image: /assets/images/homeautomation.jpg
  overlay_filter: 0.4
  overlay_height: 800
  caption: "Photo credit: [Bence Boros](https://unsplash.com/photos/anapPhJFRhM)"
classes: wide
permalink: /home-automation/
---

## Home Assistant Posts

{% assign entries_layout = page.entries_layout | default: 'list' %}
<div class="entries-{{ entries_layout }}">

    {%- for post in site.posts -%}
        {% if post.tags contains 'home assistant' %}
            {%- unless post.hidden -%}
                {% include archive-single.html type="grid" %}
            {%- endunless -%}
        {% endif %}
    {%- endfor -%}

</div>




 
