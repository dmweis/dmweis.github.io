---
title: "Week Three: Tracking"
categories: MIX
---

# This week’s topic is Tracking!

Let me start by saying that tracking is my most favorite topic in VR/AR. And when I started writing this post the first time, I wrote 700 words of introduction alone. I may try to post it as an alternative version one day, but for now, here is a very compressed version!

We didn't do a full project this week. At least not entirely. Instead our [Mobile VR project]({{ site.baseurl }}{% post_url 2017-03-15-WeekFour-Mobile VR %}) is a bit of a combination of tracking and mobile. But let me start from the beginning.
As I said tracking is my favorite topic. And my most favorite tracking system is the Lighthouse tracking system used by the HTC Vive. It was one of the driving reasons for why I bought a Vive for myself. And I must say that my Vive spends most of its days just laying on my table and plugged in just to work as a receiver for my controllers. Wiring a simple program that circumvented the entire VR system and just used to controllers as tracking points was actually one of the first things I did with my Vive. And I have since used it for many things ranging from controlling a robotic arm to copy motion of human hand to room mapping using a combination of a Kinect sensor and a controller.

Simple code for getting data from the Lighthouse tracking system:
```csharp
static void ReadRelativePosition()
      {
         EVRInitError initError = EVRInitError.None;
         CVRSystem cvrSystem = OpenVR.Init(ref initError, EVRApplicationType.VRApplication_Utility);
         Console.WriteLine("Error: " + initError.ToString());
         if (cvrSystem == null)
         {
            Console.WriteLine("Error! evrSystem null!");
         }
         while (true)
         {
            TrackedDevicePose_t[] trackedDevicePose = new TrackedDevicePose_t[OpenVR.k_unMaxTrackedDeviceCount];
            cvrSystem.GetDeviceToAbsoluteTrackingPose(ETrackingUniverseOrigin.TrackingUniverseRawAndUncalibrated, 0f, trackedDevicePose);
            VRControllerState_t controllerState = new VRControllerState_t();
            cvrSystem.GetControllerState(1, ref controllerState, (uint) System.Runtime.InteropServices.Marshal.SizeOf(typeof(VRControllerState_t)));
            bool trigger = controllerState.rAxis1.x > 0.9f;
            bool applicationButton = (controllerState.ulButtonPressed & (1ul << (int)EVRButtonId.k_EButton_ApplicationMenu)) != 0;

            TrackedDevicePose_t pose = trackedDevicePose[1];
            ETrackingResult trackingResult = pose.eTrackingResult;
            HmdMatrix34_t hmdMatrix = pose.mDeviceToAbsoluteTracking;
            Position pos = new Position(hmdMatrix); // This object calculates position from the hmdMatrix
            Rotation rot = new Rotation(hmdMatrix); // This object calculates rotation from the hmdMatrix
            Console.WriteLine($"Position: {pos} Rotation: {rot} trigger {trigger} app {applicationButton}");
         }
      }
```
[Link to repository on my Github](https://github.com/dmweis/SteamVrTest)

Let me explain why I love the Lighthouse first. The Lighthouse system works on a very simple principle that is based on the system used to navigate plans for landing. Ironically something I become very acquainted with when I started working for Insero Software and was on a team that wrote an application used for testing of just that system.
The Lighthouse consists of one/two base stations that have infrared lights in them that regularly flash the entire room with infrared light, and then two flywheels that do a horizontal and vertical sweep with infrared light. The tracked objects in turn have multiple sensors that detect when they are hit with infrared light. And since we know the frequency at which the flywheels rotate and the exact position of the sensors on the tracked device we can triangulate the exact pose of each device.
Since I am limited on word count I won't go into detail but here is a nice video that shows the system at work.

<iframe width="560" height="315" src="https://www.youtube.com/embed/J54dotTt7k0" frameborder="0" allowfullscreen></iframe>

This system may seem complicated at first. Especially since it has so many moving parts. Contrary to the Constellation tracking system used by Rift for example. But the magical thing about Lighthouse is that in my experience. It just works and is very simple to use. And requires minimum equipment to get a simple tracker to work. Another thing that I love about this system is that it’s almost entirely open. And very "Hackable". Especially thanks to all of the information available online thanks to one of its creators **Alan Yates**, Valves [Chief Pharologist](https://en.wikipedia.org/wiki/Pharology). who himself released a lot of information about its internal working including a schematic for a DIY Tracker circuit and made several talk on the topic of hacking it.

![Schematic for a simple Lighthouse sensor]({{site.url}}/images/MixWeekThreeTracking/lighthouse_sensor_schematic.jpg)

Since we combined this weeks project with Mobile VR I will talk about what we did in the post about [Mobile VR]({{ site.baseurl }}{% post_url 2017-03-15-WeekFour-Mobile VR %})

But before I finish this I want to talk about one interesting problem we had to solve.
Since our intention was to use an Vive controller without the headset we were facing one simple problem. The receivers for the Controllers are located [inside the headset itself](https://www.ifixit.com/Teardown/HTC+Vive+Teardown/62213#s130831). This is a smart solution because the headset is always very close to the headset and will almost never loose signal. But it was a problem for us. The simple solution is to use a SUB cable and connect the controller straight to the computer. This will cause the controller to switch to wired mode and it will work. But I didn't like the wired solution.
My first attempt was to get the same receiver chip used in the headset and simply flash it the Vive controller dongle firmware. The headset has two [Nordic Semiconductor nRF24LU1+ Transcievers](https://www.sparkfun.com/datasheets/Wireless/Nordic/nRF24LU1P_1_0.pdf "Datasheet for Nordic Semiconductor nRF24LU1+ Transceiver") it uses to communicate with the controllers. So, I ordered one from china to see if I can flash it myself. It took a while (mind you. I did this a while before this semester for a different project of mine) but the transceiver did arrive.

![Nordic Semiconductor nRF24LU1+ Transciever]({{site.url}}/images/MixWeekThreeTracking/transceiver_vive.jpg)

Sadly. As many cheap things ordered from china. It was DOA. And since I didn't want to wait any longer for another possible nonfunctional chip I ended up going for a different and much easier solution. The original Vive Pre developer prototype shipped with different controllers. Among other differences these controllers came with USB dongles to connect to the computer. And an interesting fact is that these dingle are the same as the ones using for the [Steam Controller](http://store.steampowered.com/app/353370/Steam_Controller/). And after a bit of research it became obvious that the new controllers can also use the same dongle. So I simple ordered a [Steam Controller dongle](http://store.steampowered.com/app/530260/Steam_Controller_Wireless_Receiver/). And once it arrived I flashed it with the Lighthouse Watchdog firmware. And after a bit of tinkering (a simple config file edit) my controller would connect and SteamVR would run even without the headset. I would have done this in the first place even before ordering the chip from china. But sadly, it was impossible to order the dongle for the Steam Controller on its own and I didn't want to flash the one from my controller since I was worried I wouldn't be able to flash it back (A worry of mine that proved to be true afterwards).

[Back to main page!]({{ site.url }})

Last build time: {{ site.time }}
