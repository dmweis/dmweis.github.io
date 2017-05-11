---
title: "Week Eight: Final project - Part Two"
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

Reast of the application was very simple. It's a simple WPF applications with a single grid that works as a chess board. It will open with two windows. One a WPF window that holds the map with the grid plus three markers in corners to serve as base markers taht we use to estimate the relative position of the table and a second window that shows the output from the camera for debug purposes. The applicationis using [Reactive Extension .NEt library](http://reactivex.io/) for managing the datastreams from teh tracking system and a standard [RabbitMQ.net](https://github.com/rabbitmq/rabbitmq-dotnet-client) client library for network connection.
And taht leads me to the next part of the project.

## Networking

As I meantioned in the [previous blog]({{ site.baseurl }}{% post_url 2017-05-01-FinalProject-One %}) we created two separate applications for the GM table and the VR player so that they don't have to run on the same computer. And since one of our applications was built in unity and the other in WPF we couldn't use Unity's built in netwroking system since it only works between Unity applications. Lucckyly, as with most of our previous problems, this is somehting we have deal with at work. And since we were both expirience with using [RabbitMQ](https://www.rabbitmq.com/) it was't hard to decide. Especially since RabbitMQ's own moto _"Messaging that just works"_ is such a fitting description.
Sadly, as often with Unity, many things don't just work. Same was the case with RabbitMQ's .NET client. Or at least until recently. And by recently I mean about 12 days before we started our project. Someone created a AMQP client for Unity. AMQP being one of the protocols supported by RabbitMQ.
The reason the official .NET client wouldn't work is that unity (as of version 5.5.1) still only supports .NET framework 3.5. Which is severly outdated by now. And as a result the newest RabbitMQ clinet doesn't work under Unity anymore. You could get the older version of the client and crosscompile it but you would be using an outdated version that is no longer supported and filled with possible bugs. But as I said. We got lucky. Somone else was facing the same issue we were and decided to fix it by updating the last version of RabbitMQ's .NET clien that builds under Unity with patches to get it up to date and fully working. This solution is not finished by any means but it alredy is officaly listed on RabbitMQ's website and it does containc the crosscompiled .NET client that we needed. project can be found on GitHub here [CymaticLabs Unity3D.Amqp](https://github.com/CymaticLabs/Unity3D.Amqp).
This project has a lot more functionality to it that is not done yet such a Network manager that recieves messages asynchroniously and than handles them on frameupdates to remain thread safe with teh rest of Unity's system but we didn't need any of this functionality and since most of the advanced functionality was unfinished at the time we were working on it we decided to make our own very simple solution.

Now that I explained our communication in a pretty low lever let me give you a high level overview. RabbitMQ is a message borker that allows you to send messages of any type between clients of any kind as long as they support one of RabbitMQ's wide array of supported communications protocols. I am not going to go into more detail baout how exaclty it works and what all it can do since I doubt I can do a better job of it than [RabbitMQ's own documentation](https://www.rabbitmq.com/getstarted.html).
Simple explanation is that we serilaze out objects into JSONs and send them to RabbitMQ which will than route it to any other client waiting for that kind of obejct where the message will be again deserealized and handled.

Since we started by running our system on two computers we used a RabbitMQ as a service provider called [CloudAMQP](https://www.cloudamqp.com/) which has a free tier allowing you to host a small RabbitMQ server online for free as long as you stay within [certain criteria](https://www.cloudamqp.com/plans.html).
later we transiotoned to running our own RabbitMQ server since when we went to present our game we weren't sure if we are going to have access to the internet and running a local server was very easy as well.


[Back to main page!]({{ site.url }})

Last build time: {{ site.time }}
