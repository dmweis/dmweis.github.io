---
title: "Hopper Introduction"
categories: Robotics
---

# Hopper the Hexapod Robot

If you look at my previous posts you can see my [3D printed Quadruped robot]({{ site.baseurl }}{% post_url 2017-10-06-QuadrupedTeaser %})!  
I promised I would write a more comprehensive description but never got around to doing it. Most because the project very quickly evolved past that stage and I was too busy working on it to write about it. But here is a short description:

## Original Quadruped

The original quadruped robot was the first iteration of my long-term dream to build a legged robot. It had 4 legs. And was controlled by an ASP.NET core web application running on a Raspberry Pi or a desktop computer. It was a fun project. But after a while I realized that ASP.NET core is far from the ideal platform (Even if it was fun getting it to work on a raspberry pi) and I wanted to learn more about ROS. Another part was the body. The original body was heavily inspired by the [PahntomX AX Quadruped Mark II](http://www.trossenrobotics.com/p/PhantomX-AX-12-Quadruped.aspx) by [TrossenRobotics](http://www.trossenrobotics.com/). The new body is also heavily inspired by its larger six-legged brother but this time none of the parts are directly modeled after anything.

## New design

I will make a more extensive post about each of the aspects in of the new robot in the following weeks (For real this time...)  
But here is a short summary

### New body

![Hopper]({{site.url}}/images/Robotics/HopperIntro/Hopper_hexapod_kinect.jpg)

[Hopper]({{site.url}}/images/Robotics/HopperIntro/Hopper_hexapod_kinect.jpg){: data-lightbox="hopper" data-title="Hopper the hexapod robot"}

The new body is again entirely modeled by me. But this time it's done without a base I was trying to copy. I have also learned a lot more about CAD so the design is a lot more parametric and higher quality.

The most obvious change is the addition of two more legs. The hexapod design allows more stable movement. And upgrade of the core of the robot. It now has a fully working battery bay where you can easy and quickly switch batteries. Main controller has also been now integrated inside of the main body. And as a cherry on top the top plate has screw holes and magnetic mounts for any equipment that you might want to put on top.

#### Main body of the new robot

<iframe src="https://myhub.autodesk360.com/ue280e3f5/shares/public/SHabee1QT1a327cf2b7a5264dd8a20f0d670?mode=embed" width="640" height="480" allowfullscreen="true" webkitallowfullscreen="true" mozallowfullscreen="true"  frameborder="0"></iframe><br>

#### New legs for the robot

<iframe src="https://myhub.autodesk360.com/ue280e3f5/shares/public/SHabee1QT1a327cf2b7a7cf9b78f9a8736f1?mode=embed" width="640" height="480" allowfullscreen="true" webkitallowfullscreen="true" mozallowfullscreen="true"  frameborder="0"></iframe><br>

### New software platform

I made a decision to switch my software platform to [ROS - Robot Operating System](http://www.ros.org/). I did this for multiple reasons. It's a more Linux friendly platform, it allows me to use a much more modular design, it's a lot closer to the industry standard for robotics, and it allows me to quickly and easily add new sensor and devices (Such as the Kinect sensor you can see in the image above ^) since most of them already have a working ROS driver. Another important reason for switch was that I really wanted to learn ROS and was getting bored of just using a boring car platform to experiment on.

I am very new to ROS and python and C++ as well so I don't think my work is worth much so far but you can find my entire ROS stack for my robot [here on my GitHub](https://github.com/dmweis/Hopper_ROS). It's mostly written in python with the web interfaces written in JavaScript with Node using [RosNodeJs](http://wiki.ros.org/rosnodejs). I plan on separating parts of it into multiple nodes to make it more ROS like and rewriting parts of it C++ once I feel comfortable with where they are.

### Why Hopper?

I had a hard time picking a name for my robot. I don't think I am good at naming my robots yet. Especially because I like changing my mind a lot. But in this case, I found a name I like it lot. Hopper is named after [United States Navy Rear Admiral Grace Hopper](https://en.wikipedia.org/wiki/Grace_Hopper), One of the people responsible for the first ever compiler and also a person who famously found one of the [first software bugs](https://thenextweb.com/shareables/2013/09/18/the-very-first-computer-bug/). And because of the hopping movement it does when walking.
