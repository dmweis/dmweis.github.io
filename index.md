---
title: David Weis
---

# Hey!

_This website isn't being actively maintained anymore._

I am a software engineers originally from Slovakia.  
Currently I am living in UK working on self-driving cars.

If you are interested in some of my projects I recommend that you check out my [GitHub](https://github.com/dmweis)

If you are interested in offering me a job [here is my resume]({{ '/assets/files/David_Weis_Robotics_Engineer_2024.pdf' | relative_url }}) or you can take a peek at my [LinkedIn](https://www.linkedin.com/in/david-weis/)

## Latest posts

{% for post in site.posts limit:5 %}
- [{{ post.title }}]({{ post.url }})
{% endfor %}

## [Robotics or DIY projects]({{ site.baseurl }}{% link pages/robotics_projects.md %})

You can find all of my robotics projects [here]({{ site.baseurl }}{% link pages/robotics_projects.md %})

## [MIX]({{ site.baseurl }}{% link pages/mix_posts.md %})

If you are looking for posts written for my Mixed reality Course at VIA you can find them [here]({{ site.baseurl }}{% link pages/mix_posts.md %})
