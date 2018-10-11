---
layout:     page
title:      Stories
permalink:  /stories/
---

## Short stories/chapters

{% for story in site.stories %}
  * {{ story.date | date_to_string }} &raquo; [ {{ story.title }} ]({{ story.url }})
{% endfor %}
