---
title: David Weis
---

# Hello, I'm David!

I am a Software Engineer from Slovakia, currently residing in Denmark.  
I work in Air Traffic Management, writing software that makes sure planes don't fall during the day and building robots during the night.  

This site doubles as a personal blog and a light portfolio but it's current only updated sporadically (Mostly whenever I get bored...)  

If you are interested in some of my projects I recommend that you check out my [github](https://github.com/dmweis)  

Or if you are interested in offering me a job or checking out what am I up to these days you can take a peek at my [LinkedIn](https://www.linkedin.com/in/david-michael-weis/)

## Latest post

- [{{ site.posts.first.title }}]({{ site.posts.first.url }})

## Robotics projects

{% for post in site.categories.Robotics %}
- [{{ post.title }}]({{ post.url }})
{% endfor %}
