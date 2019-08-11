# Making legged robots climb things

## Intro

### Problem formulation

The point of this project was to develop a novel solution to obstacle traversal for legged robots. This project was furthure specifalized by using a self implemented platform and a set of low cost sensors.

### Choice of platform

CHoice of platform was an importatnt step of this project. THe original intention was to use the Anymal robot by ETH Zurich. This prooved to be inconvinient and quite limiting which forced the project to use a different paltform.

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


There are other features that aren't documented here but worht mentioning:

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

##### Tibia finger switches

The robot needed a mecahnism for detecting is the toe (tip of the tibia) was making contact with ground. Since I was going for a low cost and simple solution I chose to use simple momentary microswitches. These are easy to get and very easy to integrate. They only provide digital feedback (Touching/not touching) instead of a detailed analogue sensor that would provide continuouse signal showing pressure. But presure sensors on this scale are expensive and difficult to get.

<details>
    <summary>CAD models and pictures</summary>
    Original Tibia design:
    <iframe src="https://myhub.autodesk360.com/ue280e3f5/shares/public/SHabee1QT1a327cf2b7a972613eebcb14c45?mode=embed" width="640" height="480" allowfullscreen="true" webkitallowfullscreen="true" mozallowfullscreen="true"  frameborder="0"></iframe>  

    New tibia design with a place for a switch:
    <iframe src="https://myhub.autodesk360.com/ue280e3f5/shares/public/SH919a0QTf3c32634dcf8cffdd00cbfe5ef9?mode=embed" width="640" height="480" allowfullscreen="true" webkitallowfullscreen="true" mozallowfullscreen="true"  frameborder="0"></iframe>  

    Image of foot with sensor
    [![Foot Sensor]({{site.url}}/images/Robotics/HopperMasters/FootSensor.JPG)]({{site.url}}/images/Robotics/HopperMasters/FootSensor.JPG){: data-lightbox="Hopper foot sensor" data-title="Hopper foot sensor"}

    The switches are wired into an Arduino NANO microcontroller on the back of the robot that pushes the data from them into the main computer using a simple serial connection

    [![foot switch controller]({{site.url}}/images/Robotics/HopperMasters/SwitchController.JPG)]({{site.url}}/images/Robotics/HopperMasters/SwitchController.JPG){: data-lightbox="foot switch controller" data-title="foot switch controller"}
</details>

##### Cooling

One of the problems encountered during the project was that once most of the processing was moved to the on board comptuer it started overheating.
As most modern computers, the Raspberry pi, will throttle it's CPU when it detects overheating. This caused the gait generation to slow down and hoppers movement would start stuttering.

The solution to this problem was to add active cooling to the robot. Since I didn't want to deal with these issues ever again I decided to add 3 40mm fans that would push air inside of the main body and cooling fins that would allow the air to escape. This way I created positive presure inside of the main body which meant that air would flow inside before escaping through any opening. This will also prevent the gathering of dust in the main body.

The cooling fans are clearly visible in the main CAD model. One on top of each middle leg and one on top of the head.

<details>
    <summary>Pictures</summary>

    Image of foot with sensor
    [![Main cooling fan]({{site.url}}/images/Robotics/HopperMasters/CoolingFanHead.JPG)]({{site.url}}/images/Robotics/HopperMasters/CoolingFanHead.JPG){: data-lightbox="Main cooling fan" data-title="Main cooling fan"}

    [![Side cooling fan]({{site.url}}/images/Robotics/HopperMasters/CoolingFanSide.JPG)]({{site.url}}/images/Robotics/HopperMasters/CoolingFanSide.JPG){: data-lightbox="Side cooling fan" data-title="Side cooling fan"}
</details>

### Software

### Results


### Testing


### Problems

### Future development


## Conclusion

### Closing statement
