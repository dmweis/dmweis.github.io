---
title: "Quadruped Teaser"
categories: PrivateProjects
---

# Quadruped teaser!

_This is just a small teaser of my current project. There will be another post in the next few weeks where I will go into detail_

This project was inspired by my constant fascination with walking robots. I always wanted to build one but a two legged robot is a pretty big challenge. Hexapods are a bit easier but with 18 motors just for 3 degrees of freedom per leg they can get pretty pricey. Especially since I decided to use good motors which cost a bit more. So in the end I went with a quadruped. With 3D printed parts.

The design is heavily inspired by [PahntomX AX Quadruped Mark II](http://www.trossenrobotics.com/p/PhantomX-AX-12-Quadruped.aspx) by [TrossenRobotics](http://www.trossenrobotics.com/)

For motors I used the [Dynamixel AX-12A](http://www.robotis.us/ax-12a/) from [ROBOTIS][1]

My current version is using a USB controller for the Dynamixel motors. The application is written in C# and running on a Windows machine. This is just a temporary solution until I figure things out. I plan on moving the entire system to a OpenCM microcontroller also from [ROBOTIS][1] or a [Raspberry Pi](https://www.raspberrypi.org/) running [ROS](http://www.ros.org/).

![Quadruped]({{site.url}}/images/PrivateProjects/quadruped_simple_image.jpg)

## Quadruped controlled with an Xbox controller

<iframe width="560" height="315" src="https://www.youtube.com/embed/DRt5kPq_FkQ" frameborder="0" allowfullscreen></iframe>

## Short demo of the robot walking

<iframe width="560" height="315" src="https://www.youtube.com/embed/A0_89ODIW2Q" frameborder="0" allowfullscreen></iframe>

## Robot being controlled by a Vive Controller

<iframe width="560" height="315" src="https://www.youtube.com/embed/AjRtbsR_hW8" frameborder="0" allowfullscreen></iframe>

## Robot being controlled by a [Leap Motion](https://www.leapmotion.com/) Controller

<iframe width="560" height="315" src="https://www.youtube.com/embed/XQNM-JmyYfI" frameborder="0" allowfullscreen></iframe>

## Old demo of the robot walking. This was just following a predefined set of motions.

<iframe width="560" height="315" src="https://www.youtube.com/embed/n6QkdBZH_nw" frameborder="0" allowfullscreen></iframe>

All of the code is open source and can be found [here](https://github.com/dmweis/DynamixelServo)

I also plan on releasing the models once I am satisfied with them.

[Back to main page!]({{ site.url }})

Last build time: {{ site.time }}

[1]:http://www.robotis.us/
