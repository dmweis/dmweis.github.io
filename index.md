---
title: David Weis MIX devblog
---

# David Weis MIX devblog

This is my personal blog for our MIX course taught at VIA university college in 2017.

### MIX posts:

{% for post in site.categories.MIX reversed %}
- [{{ post.title }}]({{ post.url }})
{% endfor %}

### Free time projects:

{% for post in site.categories.PrivateProjects %}
- [{{ post.title }}]({{ post.url }})
{% endfor %}


Last build time: {{ site.time }}

<meta name="robots" content="noindex">
