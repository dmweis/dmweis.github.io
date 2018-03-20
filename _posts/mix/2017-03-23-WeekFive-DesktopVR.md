---
title: "Week Five: Desktop VR"
categories: MIX
---

# This week's topic: Desktop VR

Finally. Desktop VR. A topic very close to my heart. Maybe not my most favorite to talk about (That would be [tracking]({{ site.baseurl }}{% post_url 2017-03-02-WeekThree-Tracking %})) but definitely one of the most favorite ones taht I also have most expirience with. As I previously mentiond I own an [HTC Vive](https://en.wikipedia.org/wiki/HTC_Vive). And I've spent countless hours with it either playing games or developing small VR expiriences. And I have to say that VR is amazing but is currently sadly limited by the limited amount of expiriences available.

# This week's project

our project this week was a bit different. Insted of focusing on the newest of the newest. We went with the polar opposite.

Enter the Victormaxx Cybermaxx:
![Victormaxx Cybermaxx]({{site.url}}/images/WeekFiveDesktopVr/cybermaxx_promo.jpg)

This headset is as 90's as it get's. Including severaf floppy disks with games

![Floppy]({{site.url}}/images/WeekFiveDesktopVr/cybermaxx_promo.jpg)

And we decided to make it run in Unity. Or at least as much of it as possible.
The headset has an RS-232 serial port for the orientation sensors and several different video inputs including VGA adn RCA.

Interestingly enought the video worked imediatly with the official supported games even when they were running in [DOSBox](https://www.dosbox.com/)

![Doom running on the headset]({{site.url}}/images/WeekFiveDesktopVr/cybermaxx_doom.jpg)

The sensory and tracking sadly didn't work since DOSBox didn't have access to the Serial Ports connected to the computer.

Next step was to try and read from the Seril Port. Luckily enought connecting to devices over SerialPorts, RS-232 specifically, is somehting I have done may times so getting a simple connection was easy.
Discovering the protocl was the hard part. But after some thorough googling we found a random repository that had a driver for the serial interface.

[Link to repository](https://stuff.mit.edu/afs/sipb/user/frodo/Thesis/CTdisk/WinTrak%20II/MAXXCOM.H)

and from there it became very easy to write a simple C# script that read the input and get it to work in Unity


[Back to main page!]({{ site.url }})

Last build time: {{ site.time | date_to_long_string: "ordinal" }}
