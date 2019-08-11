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



## Implementation

### Hardware

### Software

### Results


### Testing


### Problems

### Future development


## Conclusion

### Closing statement
