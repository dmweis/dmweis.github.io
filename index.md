---
title: David Weis
---

# Hi, I'm David!

*Welcome to my new revamped page!!!*

I am a Software Engineer from Slovakia, currently residing in Scotland.  
I used to be a software engineer working in Air Traffic control in Denmark, but I got bored with my job one day and decided to move to Edinburgh and get a degree in Robotics instead. Now most of my days consist of skipping classes, teaching workshops and building robots.  

~~This site doubles as a personal blog and a light portfolio, but it's current only updated sporadically (Mostly whenever I get bored...)~~  
^ is what the site used to say, and it's still very much true, but I am working on the sporadically part.

If you are interested in some of my projects I recommend that you check out my [GitHub](https://github.com/dmweis)  

Or if you are interested in offering me a job or checking out what am I up to these days you can take a peek at my [LinkedIn](https://www.linkedin.com/in/david-weis/)

## Latest posts

{% for post in site.posts limit:5 %}
- [{{ post.title }}]({{ post.url }})
{% endfor %}

## Robotics or DIY projects

You can find all of my robotics projects [here]({{ site.baseurl }}{% link pages/robotics_projects.md %})

## Where is David?

This project was put on hiatus. But if you'd like to check where I am anyway you can have some luck [here](http://aprs.fi/?call=2M0WUE)

## MIX

If you are looking for posts written for my Mixed reality Course at VIA you can find them [here]({{ site.baseurl }}{% link pages/mix_posts.md %})
