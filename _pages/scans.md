---
layout: archive
title: "Scans from the library"
permalink: /scans/
author_profile: true
---

{% include base_path %}

Papers I could not track down online.

{% for post in site.scans %}
  {% include archive-single.html %}
{% endfor %}
