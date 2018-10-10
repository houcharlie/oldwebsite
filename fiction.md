---
layout:     page
title:      Fiction
permalink:  /fiction/
---

## Short stories/chapters

{% for post in site.stories %}
  * {{ post.date | date_to_string }} &raquo; [ {{ post.title }} ]({{ post.url }})
{% endfor %}
