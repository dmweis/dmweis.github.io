---
title: David Weis MIX devblog
---

# David Weis MIX devblog

This is my personal blog for our MIX course taught at VIA unityversity college in 2017.

### MIX posts:

{% for post in site.categories.MIX %}
- [{{ post.title }}]({{ post.url }})
{% endfor %}


Last build time: {{ site.time }}
