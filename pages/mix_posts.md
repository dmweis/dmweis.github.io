---
title: MIX posts
---

# Posts written for my Mixed Reality Course

{% for post in site.categories.MIX reversed %}
- [{{ post.title }}]({{ post.url }})
{% endfor %}
