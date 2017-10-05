---
title: "Quadruped"
categories: PrivateProjects
---

# Quadruped and how I got to making it

## About the project

Building a walking robot of my own has been a dream of mine for a while. Only reason I didn't do it much earlier is that it always seemed like a pretty large project both timewise and financially. I was also worried that since this project would take a while I might get bored of it and move on so I was hesitant investing so much into a projects that may take months to finish.  
But I ended up getting to a point where most of my other project started to seem way too easy and I wanted a bit of a challenge. I especially wanted to prove to myself that I could do a project like this entirely on my own. That means no reading other people's solutions, no libraries for any of the core functionality, or anything of that sort. And in fact that only materials I have read to help me with this project were theoretical papers on gait and walking systems.  
I am also did most of the mechanical design on my own. Not all of it since I am not a mechanical engineer and don't have enough practice with designing things that should move. My design was heavily inspired by the [PahntomX AX Quadruped Mark II][1] by [TrossenRobotics][2]. At least in the overall design. All of the parts were drawn and modeled by me using [Autodesk Fusion 360](https://www.autodesk.com/products/fusion-360/overview) and printed on my 3D printer.

## My Design

the project as still far from finished at the time of writing this post but it has been few months and I feel like I have something interesting to show at this point.  
I don't remember when exactly I started working on this, but I've been looking at Hexapods and Quadrupeds for a while and that's how I discovered the already mentioned TrossenRobotics PahntomX AX Quadruped Mark II. And I immediately fell in love with it. I was already familiar with the [Dynamixel][5] motors it was using from the research I did when building my robotic arm and I've been trying ti find a valid reason to buy a few for a while.  
My original plan was to build a Hexapod. Hexapods are easier to get started with since they have more legs that can hold the robot stable while the rest moves and they are also more stable. The problem with a hexapod is that it needs 18 motors. And buying 18 motors seemed like a lot, especially since all fo my previous projects have been very low budget. So I decided to go with a Quadruped which only needs 12 motors. I could always buy 6 more motors if I liked them and quadruped proved to be too difficult as a starting project.  

## Start

I started by ordering 4 [Dynamixel AX-12A](http://www.trossenrobotics.com/dynamixel-ax-12-robot-actuator.aspx). I wrote a simple .NET wrapper around the [DynamixelSDK driver library](https://github.com/ROBOTIS-GIT/DynamixelSDK) and built a small robotic arm with them. After few days of playing with them I decided a I like them a lot and ordered 8 more so that I could get started on the Quadruped.

## Current Status

As I said before. The robot is still far from finished. The current project is just an extended version of my original .NET driver for the servo motors. I plan on moving all of this code either to a microcontroller on the quadruped or a Raspberry Pi that can be attached on top of the robot.  
I chose to write the initial version in C# and running it from a Windows computer because C# is my most favorite language and I am most familiar with the environment and what it offers. Which means I can prototype it much faster and I will never be limited by my knowledge of the environment.  

[1]:http://www.trossenrobotics.com/p/PhantomX-AX-12-Quadruped.aspx
[2]:http://www.trossenrobotics.com/
[3]:http://www.ros.org/
[4]:http://www.robotis.us/
[5]:http://www.robotis.us/dynamixel/
[6]:http://www.turtlebot.com/