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

Connect raspberry pi to wifi
Start start openhab on pi terminal command: sudo systemctl start openhab2.service
Start the mqtt server on raspberry pi terminal command: mosquitto
Connect esp to wifi and raspberry pi, edit SSID passpoword and raspberry pi ip adress in the esp code

## Hardware design

hardware components for the system are fairly simple

You'll need to following parts:

* ESP-8266
    - You can use any board of your choice
* actuator for the window
    - We are using a TowerPro MG-945 servo in our project but depending on your needs you can use any actuator that fits your use case
* DHT-22 or DHT-11 times 2
    - Since we want to measure temperature outside and inside of the window we need at least two temperature sensors. In our case we went with a DHT-22. It's a reliable sensor, cheap and one of our team members had previous experience with using it
* Server for OpenHAB and MQTT broker
    - We used a raspberry pi as our server but at home this can be running on any computer.

Wiring diagram:

[![Circuit diagram]({{site.url}}/images/iot_window/circuit_diagram.png)]({{site.url}}/images/iot_window/circuit_diagram.png){: data-lightbox="Turtlebot" data-title="Circuit diagram"}

The prototype window we created was made out of laser cut acrylic and and few 3D printed parts.

<iframe src="https://myhub.autodesk360.com/ue280e3f5/shares/public/SHabee1QT1a327cf2b7a6b4c92f8b2722c80?mode=embed" width="640" height="480" allowfullscreen="true" webkitallowfullscreen="true" mozallowfullscreen="true"  frameborder="0"></iframe>



## Software design

System Diagram:

[![System diagram]({{site.url}}/images/iot_window/system_diagram.png)]({{site.url}}/images/iot_window/system_diagram.png){: data-lightbox="Turtlebot" data-title="System diagram"}

Entire code for the ESP can be found [here](https://gist.github.com/dmweis/b9399950ffe3b184748f0e64069d9eb9)

```c
#include <ESP8266WiFi.h>
#include <PubSubClient.h>
#include "DHT.h"
#include <Servo.h>
#define DHTPIN_in 2       //What digital pin the first DHT22 is connected to // this is pin 4 on the esp
#define DHTPIN_out 4      //What digital pin the second DHT22 is conected to // this is pin 2 on the esp
#define DHTTYPE DHT22     //DHT22 sensor
// Inside and outside sensors
DHT dht_in(DHTPIN_in, DHTTYPE);
DHT dht_out(DHTPIN_out, DHTTYPE);
Servo myservo;


//Update these with values suitable for your network.
const char* ssid = "SSID";
const char* password = "WIFI_PASSWORD";
const char* mqtt_server = "MQTT_SERVER_IP_HOSTNAME";

WiFiClient espClient;
PubSubClient client(espClient);
long lastMsg = 0;
char msg[50];
char msg1[50];
char msg2[50];
char msg3[50];
int value = 0;

void setup_wifi() {
  delay(10);
  // We start by connecting to a WiFi network
  Serial.println();
  Serial.print("Connecting to ");
  Serial.println(ssid);

  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  randomSeed(micros());

  Serial.println("");
  Serial.println("WiFi connected");
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP());
}

void callback(char* topic, byte* payload, unsigned int length) {
  Serial.print("Message arrived [");
  Serial.print(topic);
  Serial.print("] ");
  for (int i = 0; i < length; i++) {
    Serial.print((char)payload[i]);
  }
  Serial.println();

  if (strcmp(topic, "window") == 0){
    if ((char)payload[0] == '1') {
      Serial.println("Closing");
    myservo.write(180);
  } else {
      Serial.println("Opening");
    myservo.write(150);
  }
    
  }
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
      Serial.println("connected");
      client.subscribe("window");
    } else {
      Serial.print("failed, rc=");
      Serial.print(client.state());
      Serial.println(" try again in 5 seconds");
      // Wait 5 seconds before retrying
      delay(5000);
    }
  }
}

void setup() {
  Serial.begin(9600);
  setup_wifi();
  myservo.attach(13);
  myservo.write(180);
  client.setServer(mqtt_server, 1883);
  client.setCallback(callback);
  dht_in.begin();
  dht_out.begin();
  Serial.println("Device Started");
  Serial.println("-------------------------------------");
  Serial.println("Running DHT!");
  Serial.println("-------------------------------------");

}

int timeSinceLastRead = 0;
void loop() {

  if (!client.connected()) {
    reconnect();
  }
  client.loop();
    //Report every 2 seconds.
  if(timeSinceLastRead > 2000) {
    Serial.println("Starting reading!!!");
    //Reading temperature or humidity takes about 250 milliseconds!
    //Sensor readings may also be up to 2 seconds 'old' (its a very slow sensor)
    float h_in = dht_in.readHumidity();
    float h_out = dht_out.readHumidity();
    //Read temperature as Celsius (the default)
    float t_in = dht_in.readTemperature();
    float t_out = dht_out.readTemperature();
  

    //Publish inside and outside temp and humidity
    snprintf (msg, 50, "Temperature Inside: %f", t_in);
    Serial.print("Publish message: ");
    Serial.println(msg);
    client.publish("Bedroom_Temperature", msg);
    
    snprintf (msg1, 50, "Humidity Inside: %f", h_in);
    Serial.print("Publish message: ");
    Serial.println(msg1);
    client.publish("Bedroom_Humidity", msg1);

    snprintf (msg2, 50, "Temperature Outside: %f", t_out);
    Serial.print("Publish message: ");
    Serial.println(msg2);
    client.publish("Backyard_Temperature", msg2);
    
    snprintf (msg3, 50, "Humidity Outside: %f", h_out);
    Serial.print("Publish message: ");
    Serial.println(msg3);
    client.publish("Backyard_Humidity", msg3);
    timeSinceLastRead = 0;
    }
  delay(100);
  timeSinceLastRead += 100;
}
```

## Conclusion

We created an IoT system as specified in our project specification, utilising two sensors in the form of the DHT22 Temperature and Humidity Sensor and an actuator, in the form of a servo motor. The system is controlled through OpenHAB, where the user is presented with a UI that allows for a window to be opened or closed remotely depending on a desired temperature. Both temperature and humidity are displayed on the UI.

## Authors:

Christopher Brown  
Scott Mathers  
[David Weis](DavidMakesRobots.com)  
