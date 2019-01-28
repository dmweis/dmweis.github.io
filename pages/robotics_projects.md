---
title: Robotics projects
---

# Here you can find a list of my latest robotics projects

## Latest Robotics post

- [{{ site.categories.Robotics.title }}]({{ site.categories.Robotics.url }})

## Robotics projects

{% for post in site.categories.Robotics %}
- [{{ post.title }}]({{ post.url }})
{% endfor %}
