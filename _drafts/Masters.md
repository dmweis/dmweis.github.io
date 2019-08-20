# Making legged robots climb things

## Abstract

Robots suck so I make them better. Duh!

## Intro

### Problem formulation

The point of this project was to develop a novel solution to obstacle traversal for legged robots. This project was furthure specifalized by using a self implemented platform and a set of low cost sensors.

### Choice of platform

CHoice of platform was an important step of this project. THe original intention was to use the Anymal robot by ETH Zurich. This proved to be inconvinient and quite limiting which forced the project to use a different platform.

Instead this project use a self developed hexapod robot called Hopper.

#### Hopper

##### Hardware

<iframe src="https://myhub.autodesk360.com/ue280e3f5/shares/public/SHabee1QT1a327cf2b7a5bb708b659591a23?mode=embed" width="640" height="480" allowfullscreen="true" webkitallowfullscreen="true" mozallowfullscreen="true"  frameborder="0"></iframe>  

Hopper was originally designed as a private project. It's a hexapod robotic platform inspired by [TrossenRobotics](http://www.trossenrobotics.com/) [PahntomX AX Quadruped Mark II](http://www.trossenrobotics.com/p/PhantomX-AX-12-Quadruped.aspx) Its's main distonguishing feature are the motors. Robot uses 18 [Dynamixel AX-12A](http://www.robotis.us/ax-12a/). This are serially linked smart servos that allow for individual control over a message bus.  

The robot is fully manufactured using rapid prototyping methods. Main body is mostly 3D printed with the 3 main horizontal panels made out of laser cut acrylic (THese could also be 3D printed except it would take a long time). This allowed me to quickly upgrade the design as needed and made it very easy to experiment with hardware changes such as longer legs.  

Hopper is also equiped with a set of sensors and other features. Main computer is a Raspberry Pi 3 running Ubuntu.
Hopper also has:

* Camera
  * located in the center of the face. It's used for face tracking and was considered for SLAM and mapping
* Lidar
  * Mounted at the top of the robot is a 2D lidar. It's a cheaper unit producted by SlamTech [Rplidar A1m8](http://www.slamtec.com/en/lidar/a1)
  * It boasts range of more than 10 meters (heavily dependent on surfaces)
  * scanning speed of 8000 points and default rotation speed of 5 hz (This can be adjusted to change the latency of scans while sacrificing scan detail)
* IMU
  * 9 DOF (3 dof Accelerometer, 3 dof Gyroscope, 3 dof magomometer) [BNO055](https://www.bosch-sensortec.com/bst/products/all_products/bno055)
  * This imu has a built in microcontroller that runs a fusion algorithm on board
  * Also includes a low precison temperature sensor that isn't used
* Load sensors
  * Each motor can measure torque load. This measurement wasn't used becuase of how imprecise it is and because of how much it slows down the response speed of the motors
* Contact switches in feet
  * Added as part of the project
  * [![Foot Sensor]({{site.url}}/images/Robotics/HopperMasters/FootSensor.JPG)]({{site.url}}/images/Robotics/HopperMasters/FootSensor.JPG){: data-lightbox="Hopper foot sensor" data-title="Hopper foot sensor"}

Hopper also has other set of features not used in this project

* Face
  * Face equiped with two sets of Individually addressable RGB LEDs
  * These are used for visualizing different modes and as a primitive HRI element
* Speaker
  * Hopper is equiped with a USB speaker that can be used to play sounds
  * Speaker was originally used to do text to speech using Google's tts API (It would play messages about sensor status, battery low level warnings,...)
  * This was later changed to playing simple sounds in the style of R2D2 (People don't respond well to non humanoid robots talking to them in a familiar voice...)

###### Hopper leg design

Since the design of Hopper's legs is very iportant for this project I am dedicating an entire section to it.

Hopper is a hexapod. That means it has six legs. All of the legs are the same (And are effectively interchangable). Each leg is serially linked and has 3 degrees of freedom. Leg components are named the same way they would be on an insect. Coxa, Femur and Tibia.

####### Coxa

Closest to the body. Moves in horizontal direction.  

<iframe src="https://myhub.autodesk360.com/ue280e3f5/shares/public/SHabee1QT1a327cf2b7ac2a2ccc3016fb7e0?mode=embed" width="640" height="480" allowfullscreen="true" webkitallowfullscreen="true" mozallowfullscreen="true"  frameborder="0"></iframe>  

THe coxa compoenets consists of 5 3d printed parts. 4 identical plates that mount to the motors and 1 centeral piece that holds them together. The reason it was designed this way was for ease of printing. Iw on't go into details about how to design parts for 3D printing in thei paper but by designing parts to be printed flat against the best you get the most precise and strongest parts. Even if you add number of joints.

####### Femur 

Femur is located between the Coxa and the Tiba. It's closest respective part on a human body would be the thigh.

<iframe src="https://myhub.autodesk360.com/ue280e3f5/shares/public/SHabee1QT1a327cf2b7a1381d6e252997f18?mode=embed" width="640" height="480" allowfullscreen="true" webkitallowfullscreen="true" mozallowfullscreen="true"  frameborder="0"></iframe>  

Hopper's Femur consits of 5 3D printed parts. Same as for Coxa we sacrafice ease of assembly for manufacturing precision and strength.
The part is designed to lock around the Dynamixel AX-12 motor. The mount point for tibia is specifically moved back to allow for easy folding of the leg.

####### Tibia

The tibia is the largest part but also consistes of the simplest parts. It's the part that is making contact with the ground

<iframe src="https://myhub.autodesk360.com/ue280e3f5/shares/public/SH919a0QTf3c32634dcf8cffdd00cbfe5ef9?mode=embed" width="640" height="480" allowfullscreen="true" webkitallowfullscreen="true" mozallowfullscreen="true"  frameborder="0"></iframe>  

The end compoenent of Tibia contains the touch sensor assembly. In the originall design this was a simple rubber foot with no sensing. In a previous iteration the Tibia was angled so that it would have a better angle of attack towards the ground. But current version is straight to allow the leg to fold into the body

####### Folding

[![Original Hopper]({{site.url}}/images/Robotics/HopperMasters/OriginalHopper.jpg)]({{site.url}}/images/Robotics/HopperMasters/OriginalHopper.jpg){: data-lightbox="Original Hopper" data-title="Original Hopper"}

As you can see in the image the original hopper has a very different leg design. This design was using components from Dynamixel for the coxa part. The new hopper was redesigned to allow the robot to fold up on command. This way the robot was much easier to transport

[Video of Hopper unfolding](https://youtu.be/5bmnhgGrznc)


##### Software stack

Hopper also has a previously existing software stack implemented using [ROS](https://www.ros.org/). This stack includes:

* Velocity control of the main body with a simple static gait generator (No sensing other than current feet position)
* Body pose control
  * User can define position and orientation offset from relaxed positon and robot will maintain it
* Body pose sequencer
  * Hopper has a predefined set of motions that can be executed at will (This doesn't allow external code to define motions)
* Face tracking system
  * Hopper can use the camera in it's face to track faces and look at them
  * Face tracking is done on board of the robot to get a fast enough control loop
    * Face detection is using Haar Cascades and after initiall acuisition robot uses feature tracking to follow face around (Only way to keep a high enough tracking speed on a low powered platform)
* Text to speech and audio playbock
  * Hopper can both play audio files and translate text to speed
  * Text to speech is done using Google's Text to speech api and requires internet connection (MEssages can be cached for later use without access to the internet)
* Facial expression detection
  * Hopper can use Microsoft's [Azure face API](https://azure.microsoft.com/en-gb/services/cognitive-services/face/) to recognize faces and detect facial expressions in them. This way the robot can respond to known people and have an emotional reaction depending on their observed mood
  * (This feature isn't used anymore since I ran out of Azure cloud credit)
* Web interface
  * Hopper has a simple web interface which can be used for remote control
* SLAM and navigation
  * Hopper can use the move base stack from ROS in combination with a SLAM algorithm of choice (Currently supported Gmapping and Hectormapping) for navigation and exploratory behavior


There are other features that aren't documented here but worth mentioning:

* Web telmetry interface using NASA's openmct platform
* system telemetry for hardware and performance metrics (Important for debugging overheating issues during this project)
* Leap motion sensor integration for relative movement of body center of mass

and more...


### methodology and process

Because of the exploratory nature of this project. An my engineering background which has left me with quite a few bad expriences with waterfall methodologies and tight pre planned timelines. I choose to use a different apraoch for project planning for this project (Words more english pls!)

I choose to use a lean methodology for issue tracking. This allowed me to focus on what is always most importatnt and quickly pivot if an apraoch was working out without having to loose time replanning. Since I was alone in my team I didn't need an issues tracker to managed tasks with my teammatesa and ended up using a kanban board more as a Todo list. 

I also didn't integrate my version control systems for either software or hardware with my issue tracker since both the hardware and software design was done in the existing codebase for hopper and it's existing cad model and tracking changes never became an issue. 

I did focus on maintaining gitflow for Hopper for the course of entire project. As a result the git commit history should provide a nice timeline for the course of this project [Commit history to master](https://github.com/dmweis/Hopper_ROS/commits/master) [Branches](https://github.com/dmweis/Hopper_ROS/branches)

The mechanical desgin of hopper was also version controlled (Albeit with lesser attention to detal since I am not less skilled with CAD)

You can clearly see the adition of cooling fans in the main body design which were added as a result of the main computer overaheating during this project 

[![Mech design revision]({{site.url}}/images/Robotics/HopperMasters/HopperRevisionHistory.png)]({{site.url}}/images/Robotics/HopperMasters/HopperRevisionHistory.png){: data-lightbox="Mech design revision" data-title="Mech design revision"}

## Implementation

### Hardware

The hardware of hopper underwent 3 distinct changes to acomodate for this project

#### Better lidar mount

The original mount for the lidar on hopper was made out of componenets originally designed as a generic lidar mount and a cross piece for mounting. This design wasn't ideal as it placed the lidar rather high on top of the body. This affected both the center of mass of the robot and made scanning low obstacles harder.

A new lidar mount was design that directly integrates on the roof plate of hoppper. With no aditional connecting parts the mount is dirrectly mounted with 3 3M screws with the nuts embedded inside of the printed part.

Part can be seen here:
<iframe src="https://myhub.autodesk360.com/ue280e3f5/shares/public/SHabee1QT1a327cf2b7a0523d7005c94b391?mode=embed" width="640" height="480" allowfullscreen="true" webkitallowfullscreen="true" mozallowfullscreen="true"  frameborder="0"></iframe>  

##### Tibia toe switches

The robot needed a mecahnism for detecting is the toe (tip of the tibia) was making contact with ground. Since I was going for a low cost and simple solution I chose to use simple momentary microswitches. These are easy to get and very easy to integrate. They only provide digital feedback (Touching/not touching) instead of a detailed analogue sensor that would provide continuouse signal showing pressure. But presure sensors on this scale are expensive and difficult to get.

<details>
    <summary>CAD models and pictures</summary>
    Original Tibia design:
    <iframe src="https://myhub.autodesk360.com/ue280e3f5/shares/public/SHabee1QT1a327cf2b7a972613eebcb14c45?mode=embed" width="640" height="480" allowfullscreen="true" webkitallowfullscreen="true" mozallowfullscreen="true"  frameborder="0"></iframe>  

    New tibia design with a place for a switch:
    <iframe src="https://myhub.autodesk360.com/ue280e3f5/shares/public/SH919a0QTf3c32634dcf8cffdd00cbfe5ef9?mode=embed" width="640" height="480" allowfullscreen="true" webkitallowfullscreen="true" mozallowfullscreen="true"  frameborder="0"></iframe>  
</details>

Image of foot with sensor
[![Foot Sensor]({{site.url}}/images/Robotics/HopperMasters/FootSensor.JPG)]({{site.url}}/images/Robotics/HopperMasters/FootSensor.JPG){: data-lightbox="Hopper foot sensor" data-title="Hopper foot sensor"}

The switches are wired into an Arduino NANO microcontroller on the back of the robot that pushes the data from them into the main computer using a simple serial connection

[![foot switch controller]({{site.url}}/images/Robotics/HopperMasters/SwitchController.JPG)]({{site.url}}/images/Robotics/HopperMasters/SwitchController.JPG){: data-lightbox="foot switch controller" data-title="foot switch controller"}

##### Cooling

One of the problems encountered during the project was that once most of the processing was moved to the on board comptuer it started overheating.
As most modern computers, the Raspberry pi, will throttle it's CPU when it detects overheating. This caused the gait generation to slow down and hoppers movement would start stuttering.

The solution to this problem was to add active cooling to the robot. Since I didn't want to deal with these issues ever again I decided to add 3 40mm fans that would push air inside of the main body and cooling fins that would allow the air to escape. This way I created positive presure inside of the main body which meant that air would flow inside before escaping through any opening. This will also prevent the gathering of dust in the main body.

The cooling fans are clearly visible in the main CAD model. One on top of each middle leg and one on top of the head.

Image of foot with sensor
[![Main cooling fan]({{site.url}}/images/Robotics/HopperMasters/CoolingFanHead.JPG)]({{site.url}}/images/Robotics/HopperMasters/CoolingFanHead.JPG){: data-lightbox="Main cooling fan" data-title="Main cooling fan"}

[![Side cooling fan]({{site.url}}/images/Robotics/HopperMasters/CoolingFanSide.JPG)]({{site.url}}/images/Robotics/HopperMasters/CoolingFanSide.JPG){: data-lightbox="Side cooling fan" data-title="Side cooling fan"}

### Software

The software stack on hopper had to undergo the main changes.

Original aproach to solving our problem was to performa a 3D scan of the obstacle using the 2D lidar attached to the top of the robot. It's a pretty commong practice to obtain 3D scans of the enviroment by tilting a 2D lidar. So common in fact that ROS includes multiple libraries just for mergeing laser scans into point clouds.
Hopper even offered a unique oportunity. We didn't have to add a motor or mounting system that would allow for tilting since the hexapod legged platform can alredy move it's body in full 6 Degrees of freedom.
As previously mentioned hopper alredy had the ability to let external nodes modify it's posture. This was allowed by exposing a topic with 2 vectors as arguemnts. One for translation and one for roll, pitch and yaw in euler angles (After later usage I wish I used a quaternion for rotation instead)

#### Lidar scanning node

This node has two scanning modes.
First is a continuouse scanning mode in which hopper keeps gradualy tilts in every direction slowly following a circle. This allows the lidar to scan all of it's suroundings. The beam isn't high enough to observe tall obstacles but it allows us to create an image of the 3D space surrounding the robot. You can see a video [here](https://youtu.be/StWPPSDLb4A)

Second mode is a servic allowing for scan on demand. This service has two modes. Front/back and left/right. In front/back mode the robot tilts facting down and slowly turns it's face upwards. This way the lidar slowly scans everything in front and behind the robot. You can see video [here](https://youtu.be/HUGzx2rlzIY). The left to right mode does the same thing but from one side to the other. THis service also takes an angle (This is the vertical field of view of the scan) and a duration (This is how long the scan should last). By making the scan longer the lidar will have time to do more scans and produce higher resolution scans.

This progressive scanning method is using [Laser assembler package](http://wiki.ros.org/laser_assembler) from the ROS repositories. This package collects laser scans (data coming from 2D lidars) and on request assembles them into point clouds. It uses TF transforms to track the movement of the laser base.
In order to see the relative motion of the lidar we use the center of hoppers footprint as a stable point against which to translate all laser scans. This means that hopper can't move during the scan. If the robot were to move we would loose track of the relative position of the lidar. Odometry doesn't offer sufficient precision and hopper can't be using localization while tilting since localization relies on laser scan data connected in horizontal plane.

The Laser Assembler apraoch later proved to have a significant issue. Since all of the scanned pointclouds were moved to hoppers foot print. The resulting data lookd as if it originated from bellow hopper. This is an issue since OctoMap (mapping library I used to assemble occupancy data about the enviroment) uses the origin point and a place where to cast collision checking from. This means that the octomap wasn't updating correctly after moving around.

#### Processing point cloud data

In order to map the enviroment I had to process the point cloud data into a usable form.

I devised 3 different aproaches for this:

1. Reconstruct surface mesh out pointcloud
1. Crate probabilistic 3D map using octrees and octmap
1. detect with raw point cloud data and detect surface normals and contact points based on point density

Out of all of these OctoMap was the simplest to test. [OctoMap](http://octomap.github.io/) is a probabilistic mapping framework that uses Octrees to map the world. Octrees are a very efficient method for storing data about the world. Another major adventage of octrees over the other two apraoches was that octrees allow us to encode the difference between unknown and empty space. This is important since you need to plan movement differently around empty and unknown areas. This would also allow us to potentially move the robot around and perform another scan from a different angle to map previously unknown area. It would also let us using mapping data from outr robots operating in the same area to enhance our knowledge of the world.

##### OctoMap

Implementing [OctoMap](http://wiki.ros.org/octomap_server) was very easy since there is an already existing ROS node for it. This node simply takes in pointcloud data and creates a map from this. From this server we can pull the new probablistic model of the world as a ROS message and process it using the Octomap library.
The library allows us to interact with the octree in differnet ways. One of the simples ways is to check collisions by raycasting. 

My first step was to simple figure out if there is a tall obstacle right in front of hopper. For this purpose I created a service which will perforam raycasts from hoppers position on map directly in front and return all points which collide on a vertical line. 
This means we effecitvely get distances of object in front of hopper at different heights. Even if this doesn't allos us to extract any information about the surface of the obstacles it lets us know the height of the obstacle and distance from it. 

#### Motion control interface

Hopper originally didn't provide an external interface for controlling motions of it's limbs. An external node could theoretically try sending position of the feet to the IK controller but it's commands would be overriden by the main boddy controller. That's why it became nessesary to construct a simple to use external API for control of hopper's body.

To allow remote control I implemented a set of services exposed by the main controller node. Specifically there are 8 services.

These are:

1. move_limbs_individual
  * Control position of all limbs
1. move_body_core
  * Control position of center of body
1. move_legs_until_collision
  * Same as first one but stops movement if legs encounter an obstacle
1. read_current_leg_positions
  * Return position of feet in world coordinates
1. move_to_relaxed
  * return body to default relaxed position
1. move_legs_to_relative_position
  * control positions of all feet but relative to their previous position
1. move_legs_to_relative_position_until_hit
  * Same as previous but only move legs until they hit an obstacle
1. move_body_relative
  * Move center of body relative to it's current position

The important distincion of these services is that they let us position limbs in the tf coordinate space. That means the user doesn't have to figure out relative position of an object to hopper and then translate the foot position. The controller node will do it on it's own.

To test this functionality I implemented a high-five feature for hopper. This node will try to detect any close objects in the laser scan data coming from the lidar and tap it's foot on them. As the name sugest the main purpose of this feature is to high-five or fist bump Hopper.
[Video can be seen here](https://youtu.be/IpTWQ5nY95U)

This feature shows the precision with which hopper can control the positions of it's in relation to detected objects.
DUring tests hopper could succesfully hit a finger with it's foot.

##### Problems

When trying to reconstruct the shape of the obstacle, it very quickly became aparently that the laser scan for any flat surface isn't dense enough. This was mostly caused by the attack under which the lidar was hitting the obstacle. If obstacle was close to height of the lidar the precision would drop significantly because even a smaller angular differece would cause significant difference in results.

This meant that trying to get surface normals from neighbouring points would be imprecise.

If I had more time I would try to use octomaps ability to map unknown space and force the robot to reposition and erescan unknown areas. This could be conviniently done using the ray cast system built into octomap that can be used to detect from which point could you see the unknown space. In order to implement this I would have to resolve the issue with octomap collision checking being done from the wrong origin point.

But even with this problem, scans had enought detail on vertical walls to precisly estimate obstacle heights and shapes. The only problem was the accuracy. Even if the obstacle was reconsutructed with good detail the entire object would be shifted from real world. After an investigation it seemed that this was a combination of lidar accuracy issue, IMU precision and motor sagging.

Accuracy issues:
* Lidar
  * even if the lidar measurements exhibited a high amount of precision (difference between precisions) it exhibited an accuracy issue of about 1 cm (This is in acordance with it's specs sheet)
  * This could be caused by many internal issues with the lidar and there isn't much I could do about it
  * Ignoring this minor issue the lidar performed admirable. There was very little noise coming from the sensor and it didn't exhibit any noticable veiling glare whcih is pa pretty commong issue with Lidars

* Motor saging
  * The position of the lidar relative to the world is calculated based on position of the legs. WHen commanding the legs the robot assumes that the 3 lowest feet ar the ones making contact with the ground and calculates the current body height from there. This is only done from the desired position before it's given to the Inverse kinematic engine. This means that the real world position of the motoros will be a bit different. This is a combination of gear backlash (The servos are using plastic gears), motor controllers holding them a bit under the desired position because of the effort to counter gravity, and because of the flex from the 3d printed parts in the legs. 
  * Parts of this could be compensated by reading the motor positions and using forward kinematics to see their position. But robot would still have a minor position mistake caused by parts flexing. Using very simple measurements with a rules it seems that the robot will sag by up to 3 cm. Depending on how it stepped to the point where it's standing and how slippery the surface is.
  If the tibias aren't almost perpendicular to the ground they have a tendency to slip a bit. 

* IMU
  * Meh. It's good.

After investigating these issues I decided that I will use the precise vertical data to estimate obstacle height. And make sure that I am correctly aligned by detecting collisions with the touch sensors on the feet. This way even if the obstacle is a bit closer or futhure away than expected hopper will simply climb over it. And if it's a bit height I will simple lift the feet heighr and lower them until I hit the obstacle from the top.

#### Climbing

Since hopper was originally designed for walking on mostly flat surfaces it's legs aren't well designed for climbing. It's originally feet didn't provide that much friction and the straight profile of tibia prevented the leg from reaching high places.

[Video of hardcoded climb](https://youtu.be/viiy9UijGl4)

Keeping in spirit with trying to adopt the walking algorithm the smallest amount possible. And knowing that the obstacle detection had significant accuracy issues. I decided to start modifying the gait gneration algorithm to allow for collision detection on feet.

##### Hoppers original gait generator

The orignal gait generator on hopper is using a staticly defined tripod gait. Tripod gait works by always lifting 3 legs. Front and rear on one side and middle on the other side. This way the robot always maintain's tripod stability. That means 3 feet are always on the ground and center of mass of the robot is inside of the triangle created by the contact points with ground. This makes this gait "Statically stable". As a result you can stop the movemnt in any stage and the robot will stay stable. 
This is contrary to dynamically stable gaits which are relying on effectively falling into a step. Dynamically stable gaits are usually used by bipedal or quadrupedal robots.

The gair generation procedure for hopper can be described in few steps.

1. select feet combination for moving
1. calculate desired position by estimating traveled distance based on desired speed
1. add distance vector to position of the feet in relaxed pose (Add vector to lifted feet and subtract from grounded feet)
1. start shifting feet while lifting the selected feet. The feet are lifted by a sinsus function calculated frm the distance traveled. This way the feet are on the ground at the start and end and reach the heighest point of trajectory in the middle of travel. Grounded feet are moved with no adjustment to heigh

This algorithm has few issues. Such as the fact that lifted feet will touch ground while moving against the direction of the movemnt of the robot. This couses them to slow the gait down. This could be fixed by moving the feet a bit more forward and lowering them while pulling back. But this is complicated and would require the feet to overshoot on every move.

In order to adapt this algorithm for collision checking I replaced the sine function with a square function. So feet lift to the max height position imediatly. SHift forward while lifted and then slowly lower while checking for any collisions. This means that feet are never moving forward unless fully lifted. This way the foot won't crush into an obstacle without sensing it. That would be an issue since the legs do not have sensors from the front and wouldn't detect any other collisions. 

The process of lowering the feet is also significantly slower. This is to prevent overshooting. The feet are basically lowered by few milimiteres at a time and after each step the touch sensor is checked. 

At the start I was worried that since the switches are digital. They may triger while the foot is still not fully lowered and the robot would sag. But this never became an issue.

### Testing

In order to test Hopper's climbing ability I constructed few different obstacles out of boxes. I decided to keep it simple and use mostly flat and wide obstacles. Basically shorter stairs.


### Results

[Here is a full video of hopper performing a scan and climbing over an obstacle with multiple layers](https://youtu.be/faWG_BYd5a0)

Even if the scan is performed the data from it is only printed on screen of my laptop and I manually adjust the height of the body and leg lift height on a remote controller. Speed and direction is also manually controlled in this video. But there is not reason the same princple wouldn't work for autonomouse navigation. 

You can clearly see the gait behavior in this video.

1. 3 legs are lifted quickly
1. Lifted legs are slowly moved towards a target while the grounded leg are pushing the robot forward.
1. Legs are slowly lowered until they encounter an obstacle at which point they stop lowering

THe robot is very stable and platform stays level. This is without any compensation from the IMU. It would be very easy to make hopper level after each step. Butit was never nessasry in my testing.

`The little wobble and up and down motion is because the first obstacle was broken. It was a folded black transport box and the bottom of it was cracked and flexing under hopper`

[This video shows hopper climbing a single taller obstacle](https://youtu.be/zKntn13qdg4)

In this case there was no scan. I simply set hopper to lift it's feet 11 cm heigh. The obstacle is 9 centimeters tall. From my experiments this seems to be the heighest ibstacle that hopper can overcome. The limit seems to be the height to which hopper can lift it's legs. As you can see in the video the legs are getting as close to the body as they can. This could be compensated by moving the feet a bit furthure away from the body but this would cause them to slip if lowered to the ground. As previously mentioned, the Tibia has to be as close to perpendicular to the ground as possible to prevent slipping.

### Conclusion



### Problems

### Future development


## Conclusion

### Closing statement
