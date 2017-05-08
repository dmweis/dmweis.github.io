---
title: "Week Three: Tracking"
categories: MIX
---

# This weeks topic is Tracking!

Let me start by saying that tracking is my most favorite topic in VR/AR. And when I started writing this post the first time I wrote 700 words of just introduction. I may try to post it as an alternative version but for now here is a very compressed version!

We didn't do a full project this week. At least not entierly. Insted our mobile VR project is a bit of a combination of trackign and mobile. But let me start from the begining.
As I said tracking is my favorite topic. And my most favorite trackin system is the Lighthouse tracking system used by the HTC Vive. It was one of the driving reasons for why I bouhgt a vive for mysel. And I have to say that my Vive spends most of it's days just laying on my table and plugged in just to work as a reciever for my controllers. Wiritng a simple program that circumvented the entire VR system and just used to controllers as tracking points was actaully one of the first things I did with my vive. And I have since used it for many things ranging from controlling a robotic arm to copy motion of human hand to room mapping using a combination of a Kinect sensor and a controller.

Simple code for getting data from the Lighouse trackin system:
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


Let me explain why I love the Lighthouse first. The Lighhouse system works on a very simple principle that is based on the system used to navigate plans for landing. Ironically something I become very aquinted with when I started workign for Insero Software and was on a team that wrote an application used for testing of just that system.
The lighuse comsists of one/two base stations that have infrared lgiths in them that regularly flash the entire room with infrared light, and than two flywheels that do a horizontal and vertical sweep with infrared light. The tracked objects in turn have multiple sensors taht detect when they are hit with infrared light. And since we know the frequency at which the flywheels rotate and the exact position of the sensors on the tracked device we can triangulate the exact pose of each device.
Since I am limited on word count I won't go into detail but here is a nice video that shows the system at work.

<iframe width="560" height="315" src="https://www.youtube.com/embed/J54dotTt7k0" frameborder="0" allowfullscreen></iframe>

This system may seem pretty complicated at first. Espeicially since it has so many moving parts. Contrary to the Constelation tracking system used by RIft for example. But the magical thing about Lighouse is that in my expirience. It just works and is very simple to use. And requiers minimum equipemnt to get a simple tracker to work. Another thing that I love about this system is that it;s almost entierly open. And very "Hackable". Especially thanks to all fo the information available online thanks to one of it's creators Alan Yates who himself released a lot of information about it's internal working sincluding a schematic for a DIY Tracker circuit and made several talk on the topic of hacking it.

![Schematic for a simple Lighthouse sensor]({{site.url}}/images/MixWeekThreeTracking/lighthouse_sensor_schematic.jpg)


Since we combined this weeks project with Mobile VR I will talk about what we did in the post about [Mobile VR]({{ site.baseurl }}{% post_url 2017-03-15-WeekFour-Mobile VR %})

But before I finish this I want to talk about one interesting problem we had to solve.
SInce our intention was to use an Vive controller withouth the headset we were facing one simple problem. The recievers for the Controllers are located [inside the headset itself][1]. This is a smart solution because the headset is alwas very close to the headset and will almost never loose signal. But it was a problem for us. The sim

[1]: https://www.ifixit.com/Teardown/HTC+Vive+Teardown/62213#s130831

Last build time: {{ site.time }}
