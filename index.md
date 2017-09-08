---
title: David Weis MIX devblog
---

# David Weis devblog

## _This blog is currently not finished_

You can probably notice that many parts of this blog are still filled with Lorem Ipsum and some features may not be finished yet. That is because this blog is still w work in progress. I am still learning [Jekyll](https://jekyllrb.com/) and I also didn't figure out how I want to use this blog.
I expect it would be more developed by the end of summer of 2017. Until then please don't consider anything here a final version.

### Free time projects

{% for post in site.categories.PrivateProjects %}
- [{{ post.title }}]({{ post.url }})
{% endfor %}

### MIX posts

{% for post in site.categories.MIX reversed %}
- [{{ post.title }}]({{ post.url }})
{% endfor %}

Last build time: {{ site.time }}

<meta name="robots" content="noindex">
