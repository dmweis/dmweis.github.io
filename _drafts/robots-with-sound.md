---
title: "Robot startup with audio"
categories: Robotics
---

# How to get PulseAudio working on from systemd services

_this guide exists mostly for myself_

When running hopper from a systemd service I encountered few issues

## Serial port permissions

lazy solutions:

Add :

KERNEL=="ttyUSB[0-9]*",MODE="0666"
KERNEL=="ttyACM[0-9]*",MODE="0666"
KERNEL=="video[0-9]*",MODE="0666"

to:

/etc/udev/rules.d/50-myusb.rules


## PulseAudio

audio won't work because PulseAudio is started in user mode

[link to solution!](https://github.com/alexa-pi/AlexaPi/wiki/Audio-setup-&-debugging#pulseaudio)

PulseAudio has to be started in system wide mode and user has to be added to the correct groups

https://superuser.com/questions/989385/how-to-make-raspberry-pi-use-an-external-usb-sound-card-as-a-default

[Back to main page!]({{ site.url }})
