# David Weis MIX devblog

This is my personal blog for our MIX course taught at VIA unityversity college in 2017.

### List of posts:

{% for post in site.posts %}
- [{{ post.title }}]({{ post.url }})
{% endfor %}

Time last built: {{ site.time }}