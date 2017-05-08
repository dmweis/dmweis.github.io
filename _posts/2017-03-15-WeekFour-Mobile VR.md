---
title: "Week Four: Mobile VR"
categories: MIX
---

_Topic in this post is related to topic from week three for so you should read the [previous blog about tracking]({{ site.baseurl }}{% post_url 2017-03-02-WeekThree-Tracking %}) first._

# This week's topic: Mobile VR

This week's topic is mobile VR and our assignemnt was to create a small Mobile VR game. Sadly Mobile VR in it's currect for isn't a favorie of mine. With very limited system resources and especially very limited input and tracking abilities. I feel like Mobile VR is far from reaching it's potential. ANd what a potential it is.
I don't think it's nessesary to point out how widespread Smart phones are. Or how much more poverful smartphones have became in the last few years. So I am not really interested in discussing the potential.
Beside trying to stay as far away as possible from Google cardboard or Gear VR I alsoway wanted to show somehting with this weeks proejct. There seems to be a large misconception taht VR is based around having a headset on your head. A misconception I don't like. And so I decided taht with this weeks project we will explore potential for VR withouth the use of a headset and maybe even evade using Google Cardboard or Gear VR.

## Our idea

As I meantioned. My idea was to show Mobile VR taht was not using a headset. ANd as luck would have it I recently saw a really good VR game taht did just that. It was an asymetrical game in which you had one player using the vive headset and fighting mosnters and one or more other players that had their phones attached to special Vive tracker and used them as gun with the phones being used as screens. So you basically had a handheald viewport into the VR world.

![VR game that inspired me]({{site.url}}/images/MixWeekFourMobileVR/game_example.jpg)

So I decided to do just that. But withouth the Vive player. To make a mobile VR game where your phone was your gun.

## How we did it

Now this solution has one problem. We need positional tracking. If we were in an indeal world. We would have a Google Tango phone to work with. But since there is currently only one Google Tango phone on the market and we don't have it yet we are stuck with our own solution. And as I assume you figured out by now from reading this and the previous blog post. Our solution was to use a Vive controller. We attached a phone to a vive controller on a speciall 3D printed mount and used it for tracking. The controller is connected to a computer where a small server pulls positional and rotational data from SteamVr and sends it over TCP to a Unity game running on a phone. The setup may sound complicated but it's actaully pretty simple.

I started by designing a simple mount for the phone

![Model of the phone mount in Slic3r]({{site.url}}/images/MixWeekFourMobileVR/slic3r_screenshot.jpg "Screenshot of model of the phone mount in Slic3r")

And here is a picture of it attached to the controller. Phone goes into the top slot and can be held in with a rubber band.

![Picture of the mount attached to the controller]({{site.url}}/images/MixWeekFourMobileVR/printed_attached_holder.jpg)

Once this was done we made a very simple shooting range in unity. Just to demonstrate the shooting mechanic

![Unity screenshot]({{site.url}}/images/MixWeekFourMobileVR/unity_shootingrange_screenshot.jpg)

afterwards we just needed a very simple script in unity to connect to the server and update position of the phone based on the movement of the phone:

```csharp
public class TcpPositionalReciever : MonoBehaviour
{
   public GameObject WorldSpace;
   public string IpAddress;

   private TcpClient _tcpClient;
   private Stream _clientStream;
   private string _incoming;
   private bool _trigger;
   private bool _appButton;
   private float _timeOfLastShot;

   // Use this for initialization
   void Start () {
      _incoming = string.Empty;
      _tcpClient = new TcpClient();
      if (SystemInfo.deviceType == DeviceType.Desktop)
      {
          _tcpClient.Connect(IPAddress.Loopback, 4242);
      }
      else
      {
          _tcpClient.Connect(IPAddress.Parse(IpAddress), 4242);
      }
      _clientStream = _tcpClient.GetStream();
      _timeOfLastShot = Time.time;
   }

   void FixedUpdate()
   {
      // shoot in fixed update since bullet si a rigidBody and has phisics
      if (_trigger && Time.time - _timeOfLastShot > 0.350f)
      {
         _timeOfLastShot = Time.time;
         GameObject newObj = GameObject.CreatePrimitive(PrimitiveType.Sphere);
         newObj.name = "bullet";
         newObj.GetComponent<Renderer>().material.color = Color.red;
         newObj.transform.position = transform.position + transform.up * -0.05f;
         newObj.transform.localScale = new Vector3(0.05f, 0.05f, 0.05f);
         Rigidbody rigidBody = newObj.AddComponent<Rigidbody>();
         rigidBody.useGravity = false;
         PhysicMaterial objMaterial = newObj.GetComponent<Collider>().material = new PhysicMaterial();
         objMaterial.bounciness = 1;
         rigidBody.collisionDetectionMode = CollisionDetectionMode.Continuous;
         rigidBody.mass = 0.1f;
         rigidBody.AddForce(transform.forward * 10, ForceMode.VelocityChange);
         Destroy(newObj, 3);
      }
   }

      // Update is called once per frame
      void Update () {
         byte[] data = new byte[1024];
         _clientStream.Read(data, 0, data.Length);
         string newData = Encoding.ASCII.GetString(data);
         _incoming += newData;
         if (_incoming.Contains("S") && _incoming.Contains("E")) // check for start and end of message
         {
            int indexS = _incoming.LastIndexOf("S", StringComparison.InvariantCulture);
            int indexE = _incoming.LastIndexOf("E", StringComparison.InvariantCulture);
            if (indexE > indexS)
            {
            ParseAndMove(_incoming.Substring(indexS+1, indexE-indexS-1));
               _incoming = _incoming.Substring(indexE);
            }
            else
            {
               _incoming = _incoming.Substring(indexE);
            }
         }
      }

   private void ParseAndMove(string text)
   {
      string[] data = text.Split(null as char[], StringSplitOptions.RemoveEmptyEntries);
      float posX = float.Parse(data[0]);
      float posY = float.Parse(data[1]);
      float posZ = float.Parse(data[2]);
      float rotX = float.Parse(data[3]);
      float rotY = float.Parse(data[4]);
      float rotZ = float.Parse(data[5]);
      float rotW = float.Parse(data[6]);
      _trigger = int.Parse(data[7]) == 1;
      _appButton = int.Parse(data[8]) == 1;
      transform.position = new Vector3(posX, posY, posZ);
      transform.rotation = new Quaternion(rotX, rotY, rotZ, rotW) * Quaternion.AngleAxis(70f, Vector3.right);
      if (_appButton)
      {
         // center in world space
         WorldSpace.transform.position = transform.position + Vector3.down * 1.7f;
         Vector3 partialRotation = transform.rotation.eulerAngles;
         WorldSpace.transform.rotation = Quaternion.Euler(0, partialRotation.y, 0);
      }
   }
}
```
And the last part was a simple TCP server taht we used to send the position form the computer which can be found **[Here.](https://github.com/dmweis/SteamVrTest)**

## Conclusion

Last build time: {{ site.time }}
