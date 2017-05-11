---
title: "Week Eight: Final project"
categories: MIX
---

As prmised [last time]({{ site.baseurl }}{% post_url 2017-05-01-FinalProject-One %})! We'll talk about the GM table part of our final project.

As I meantioned last time we don't have access to a nice multi-touch interactive table which would be an ideal solution to our problem. But we figured out a nice placeholder until we can get one.

## Our solution

Ouit solution was to use Computer vision to track fiducial markers on a TV. Where TV would serve as the table. Displaying a mpa. And markers would represent the monsters.

![GM table]({{site.url}}/images/MixWeekEight/gm_table_insero.jpg)

To track the markers we used a Computer Vision library called [ArUco](http://docs.opencv.org/3.1.0/d5/dae/tutorial_aruco_detection.html) which is a very small library build on top of [OpenCV](http://opencv.org/) that makes tracking fiducial marker very easy. Since we ended up creating our application in WPF we used .NET wrapper library [ArUco.NET](https://bitbucket.org/horizongir/aruco.net) that can be downloaded from [NuGet](https://www.nuget.org/packages/Aruco.Net/). Reason we decided to go with this solution was that I was alredy expirience with ArUco since I succesfully used it in previous prrojects

<iframe width="560" height="315" src="https://www.youtube.com/embed/-u97_TDxADw" frameborder="0" allowfullscreen></iframe>

and thanks to that it took us less than few hours to get a very simple tracking up and running.

<iframe width="560" height="315" src="https://www.youtube.com/embed/mjx6otLT_MM" frameborder="0" allowfullscreen></iframe>





[Back to main page!]({{ site.url }})

Last build time: {{ site.time }}
