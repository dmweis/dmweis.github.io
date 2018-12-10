---
title: IOT Window
permalink: /window
---

# IOT window

## Instructions for use

1. Open open hab website the Ip adress for the raspberry pi that is hosting the site
1. Go into basic UI
1. Select the home sitemap
1. Click on bedroom where the window is located 
1. Click the window switch to open and close window

## Instructions for building



## Hardware design

hardware components for the system are fairly simple

You'll need to following parts:

* ESP-8266
    - You can use any board of your choice
* actuator for the window
    - We are using a TowerPro MG-945 servo in our project but depending on your needs you can use any actuator that fits your usecase
* DHT-22 or DHT-11 times 2
    - Since we want to measure temperature outside and inside of the window we need at least two temperature sensors. In our case we went with a DHT-22. It's a reliable sensor, cheap and one of our team members had previous experience with using it
* Server for OpenHAB and MQTT broker
    - We used a raspberry pi as our server but at home this can be running on any computer.

The prototype window we created was made out of laser cut acrylic and and few 3D printed parts.

<iframe src="https://myhub.autodesk360.com/ue280e3f5/shares/public/SHabee1QT1a327cf2b7a6b4c92f8b2722c80?mode=embed" width="640" height="480" allowfullscreen="true" webkitallowfullscreen="true" mozallowfullscreen="true"  frameborder="0"></iframe>



## Software design

[![System diagram]({{site.url}}/images/iot_window/system_diagram.png)]({{site.url}}/images/iot_window/system_diagram.png){: data-lightbox="Turtlebot" data-title="Custom Turtlebot 3 front"}

## Authors:

Christopher Brown
Scott Mathers
[David Weis](DavidMakesRobots.com)
