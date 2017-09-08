---
title: "Project updates"
categories: PrivateProjects
---

# Projects I am working on right now

I've been pretty busy in the past few months. Work and bachelor project and teaching on the side seem to take up pretty much all of my free time. So here is a short update on some of the projects I have been working on whenever I found some time in between.

## Quadruped

This project was inspired by my constant fascination with walking robots. I always wanted to build one but a two legged robot seems like a pretty big challenge. Hexapods seem a lot easier but with 18 motors just for 3 degrees of freedom they can get pretty pricey. Especially since I decided to use good motors and those are inherently also a bit more expensive. So in the end I went with a quadruped. With 3D printed parts.

The design is heavily inspired by [PahntomX AX Quadruped Mark II](http://www.trossenrobotics.com/p/PhantomX-AX-12-Quadruped.aspx) by [TrossenRobotics](http://www.trossenrobotics.com/)

and for motors I used the [Dynamixel AX-12A](http://www.robotis.us/ax-12a/) from [ROBOTIS][2]

My current version is suing a USB controller for the Dynamixel motor and the application is written in C# and runnign on a Windows machine. This is just a temporary solution until I figure things out and plan on moving the entire system the the OpenCM platform also from [ROBOTIS][2] with maybe a Raspberry Pi running ROS on top of it for control and navigation 

![Quadruped]({{site.url}}/images/PrivateProjects/quadruped_simple_image.jpg)

## Turtlebot / ROS platform

My other project was created a bit on a whim. I wanted to play with depth sensing cameras and [ROS][1] but I didn't really have a chassis that could support a Kinect and a large enough power source. So I decided to build a copy of the new [Turtlebot][3] from [ROBOTIS][2].
Since [Turtlebot 3](http://turtlebot3.robotis.com/en/latest/) wasn't official released at the time I started working on this. I had a bit of trouble getting models for the plates. There was an open repository but the models in there weren't ready for 3D printing. But after some searching and modifications of the STL files I managed to get a pretty decent model that also prints really well. Some of the parts for a full Turtlebot were also a bit too expensive for me so I want with cheaper motors, an old Kinect v1 I had laying around instead of a real sense camera, no Lidar. Yet! and only a [Raspberry Pi 3](https://www.raspberrypi.org/) to power it. It's not fully working yet. But here is the prototype for now!

![Turtlebot 3 custom]({{site.url}}/images/PrivateProjects/turtlebot_ros_base_kinect.jpg)


[1]:http://www.ros.org/
[2]:http://www.robotis.us/
[3]:http://www.turtlebot.com/