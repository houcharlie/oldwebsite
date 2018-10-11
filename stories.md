---
layout:     page
title:      Stories
permalink:  /stories/
---

## Short stories/chapters

{% for post in site.stories %}
  * {{ post.date | date_to_string }} &raquo; [ {{ post.title }} ]({{ post.url }})
{% endfor %}
