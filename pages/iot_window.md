---
title: IOT Window
permalink: /window
---

# IOT window

We have decided to design and make an IoT window. The system utilises an ESP 8266, along with two DTH22 Temperature and Humidity sensors, and one servo. The system is controlled through a UI in OpenHAB, where temperature and humidity, both inside and outside, are displayed in the UI. The user can then open and close the window depending on this temperature, with use of the servo.

## Instructions for use

1. Open open hab website the Ip address for the raspberry pi that is hosting the site
1. Go into basic UI
1. Select the home sitemap
1. Click on bedroom where the window is located 
1. Click the window switch to open and close window

## Instructions for building

1. Connect raspberry pi to WiFi
1. Start OpenHAB on pi terminal command: `sudo systemctl start openhab2.service`
1. Start the mqtt server on Raspberry Pi terminal command: `mosquitto`
1. Connect ESP to WiFi and Raspberry Pi, edit SSID password and Raspberry Pi hostname address in the code

## Hardware design

hardware components for the system are fairly simple

You'll need to following parts:

* ESP-8266
  - You can use any board of your choice
* Actuator for the window
  - We are using a TowerPro MG-945 servo in our project but depending on your needs you can use any actuator that fits your use case.
* DHT-22 or DHT-11 x 2
  - Since we want to measure temperature outside and inside of the window, we need at least two sensors. In our case we went with a DHT-22. It's a reliable, cheap sensor  and one of our team members had previous experience with using it.
* Server for OpenHAB
  - We used a Raspberry Pi as our server but at home this can be running on any computer.
* MQTT broker
  - We decided to use Eclipse Mosquitto for our broker since it's light weight. We ran it on the same Raspberry Pi as OpenHAB

Wiring for the ESP is simple. We only have two sensors that connect to one digital pin and 5/3.3 V power each. On top of that we have a servo. THe servo is connect to power coming from the board and one PWM capable pin on the ESP.

Wiring diagram:

[![Circuit diagram]({{site.url}}/images/iot_window/circuit_diagram.png)]({{site.url}}/images/iot_window/circuit_diagram.png){: data-lightbox="Turtlebot" data-title="Circuit diagram"}

The prototype window we created was made out of laser cut acrylic and and few 3D printed parts. All modeled in fusion 360

<iframe src="https://myhub.autodesk360.com/ue280e3f5/shares/public/SHabee1QT1a327cf2b7a6b4c92f8b2722c80?mode=embed" width="640" height="480" allowfullscreen="true" webkitallowfullscreen="true" mozallowfullscreen="true"  frameborder="0"></iframe>

## Software design

System Diagram:

[![System diagram]({{site.url}}/images/iot_window/system_diagram.png)]({{site.url}}/images/iot_window/system_diagram.png){: data-lightbox="Turtlebot" data-title="System diagram"}

The only component in our system that required custom code was the ESP-8266. It is regularly reading temperature and humidity from the two sensors and publishing it to MQTT. THe ESP is also subscribing to MQTT commands to open the window and moves the servo motor to either open or close to window. THe window currently only supports two positions but in the future could be expanded so that the user can control how much is the window open.

The following code shows how the ESP connects to WiFi and the MQTT broker. We used a library called PubSubClient to do this.

```c
#include <ESP8266WiFi.h>
#include <PubSubClient.h>

//Update these with values suitable for your network.
const char* ssid = "SSID";
const char* password = "WIFI_PASSWORD";
const char* mqtt_server = "MQTT_SERVER_IP_HOSTNAME";

WiFiClient espClient;
PubSubClient client(espClient);

void setup_wifi() {
  delay(10);
  // We start by connecting to a WiFi network
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  randomSeed(micros());
}

void callback(char* topic, byte* payload, unsigned int length) {
  // Here we can process the incoming messages
}

void reconnect() {
  //Loop until we're reconnected
  while (!client.connected()) {
    Serial.print("Attempting MQTT connection...");
    //Create a random client ID
    String clientId = "ESP8266Client-";
    clientId += String(random(0xffff), HEX);
    //Attempt to connect
    if (client.connect(clientId.c_str())) {
      // subscribing to messages we are interested in
      client.subscribe("window");
    } else {
      // Wait 5 seconds before retrying
      delay(5000);
    }
  }
}

void setup() {
  Serial.begin(9600);
  setup_wifi();
  client.setServer(mqtt_server, 1883);
  client.setCallback(callback);
}

void loop() {

  if (!client.connected()) {
    reconnect();
  }
  client.loop();
    //Report every 2 seconds.
  if(timeSinceLastRead > 2000) {

    client.publish("test_topic", "Test message");
    timeSinceLastRead = 0;
    }
  delay(100);
  timeSinceLastRead += 100;
}
```

Full code for the ESP, including measuring temperature and moving the window actuator, can be found [here](https://gist.github.com/dmweis/b9399950ffe3b184748f0e64069d9eb9)

## Conclusion

We created an IoT system as specified in our project specification, utilising two sensors in the form of the DHT22 Temperature and Humidity Sensor and an actuator, in the form of a servo motor. The system is controlled through OpenHAB, where the user is presented with a UI that allows for a window to be opened or closed remotely depending on a desired temperature. Both temperature and humidity are displayed on the UI.

# Gallery:

Some images of the finished prototype

[![window]({{site.url}}/images/iot_window/window_1.jpg)]({{site.url}}/images/iot_window/window_1.jpg){: data-lightbox="Turtlebot" data-title="window"}
[![window]({{site.url}}/images/iot_window/window_2.jpg)]({{site.url}}/images/iot_window/window_2.jpg){: data-lightbox="Turtlebot" data-title="window"}
[![window]({{site.url}}/images/iot_window/window_3.jpg)]({{site.url}}/images/iot_window/window_3.jpg){: data-lightbox="Turtlebot" data-title="window"}

Videos of the window in action:

<iframe width="560" height="315" src="https://www.youtube.com/embed/QKd7CLUgZbE" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/3Y3uXOKx1g8" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Authors:

Christopher Brown  
Scott Mathers  
[David Weis](/)  
