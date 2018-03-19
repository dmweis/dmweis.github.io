---
title: David Weis
---

# David Weis

Hello!  
Welcome to my personal site!  
This site doubles as a personal blog/portfolio but it's current only updated sporadically (Whenever I get bored or feel like I having posted in a while)  
If you are interested in some of my projects I recommend that you check out my [github](https://github.com/dmweis)  
Or if you are interested in offering me a job or checking out what am I up to these days you can take a peek at my [LinkedIn](www.linkedin.com/in/david-michael-weis)

## Latest post

- [{{ site.posts.first.title }}]({{ site.posts.first.url }})

## Robotics projects

{% for post in site.categories.Robotics %}
- [{{ post.title }}]({{ post.url }})
{% endfor %}

## Posts written for my Mixed Reality Course

{% for post in site.categories.MIX reversed %}
- [{{ post.title }}]({{ post.url }})
{% endfor %}

Last build time: {{ site.time }}
