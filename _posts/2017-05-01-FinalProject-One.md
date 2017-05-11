---
title: "Week Seven: Final project - Part One"
categories: MIX
---

This week marks the end of regular projects every week. Insted we started working on our final project.
Our final project should show how much and what have we learned during this semester. Our main task is to create a mixed realitiy expirience of some sort. With the suggestion being that's it's a game of some sort.
My original idea was to do something with #d scanning and projection mapping since it's a technology I wanted to play with for a while. But after some discussion and brainstorming we came up with a much better and more fitting idea. An asymetrical mulitiplayer game.

Let me start by saying that both of us are avid tabletop RPG players. And we both really like the interaction between a Game Master and a player in a Role Playing game.
For those not familiar with this style of games.
Game usually consistes of one person, The Game Master, or just GM, and one or more players who play heros.
GM will create a world and story and will serve as some sort of a storyteller while the heroes create their characters and then roleplay them in the world GM created.
![DnD]({{site.url}}/images/MixWeekSeven/dnd.jpg)
Game is usually agumented by a complexe set of rules and dice which are used as a randomness generator to determin success rate of players actions.

With this idea in mind we created a very simple concept. One player, playing as the GM, or in our game Wizard, would have a monsters and a map. And he could move monsters around one a map. At the same time, the ohter player, playing as the Hero, would be weraing a VR headset and be completly imersed in the virtual world fighting the monsters controlled by the Game Master.
Simple concept. But how would we go about making somehting like this?

I'll talk a lot more in detail about how we did it and the chalenges we faced doing this in the next two blogs but first let me sketch out what are we actaully trying to build.

### GM

One of our key desires was to make the expirience for the GM as much bound in phisical world as possible. That's why we decided to make the mosnters actaully phisicsl objects that the player could move around.
It would have been obviously much easier to make th GM expirience a simple Tablet application where he can drag the monsters around on a touch screen and be done with it. But we don't do simple.

Idealy the GM would be using some sort of an interactive table. SOmething like [ReacTIVision](http://reactivision.sourceforge.net/) based multi-touch table with the monsters represented by tokens that the player could simply move around the table. And if you do a short search online you'll find out that we are by far not the first ones who thouth of doing something like this.
But sadly we don't have access to a table like that, yet, so we had to do with a custom solution that I'll talk about in [next post]({{ site.baseurl }}{% post_url 2017-05-05-FinalProject-Two %})

### Hero

The side of the hero is much more straight forward. At least for someone who has made VR games before. Player simply puts on a VR headset, an HTC vive in our case, and fights tho monsters. SInce we only have few weeks I doubt we'll create an complexe mechanics and won't go far beyond the very standard teleport around, shoot fireballs mechanics taht are almost standard for VR games nowadays(if you can call VR games standard alredy) but the point of the project is not to create a perfect VR game but a proof of concept asymetrical game where the interesting part should be interaction between the Two players rather than the mechanics of the VR game. Especially since this part can always be enhanced afterwards if the concept prooves interesting.

### Glueing it togheder

This proved to be a bit of a problem. Especially since we decided to create two separate applications for the GM and Hero part of the game. But luckyly both of us alredy had expirience with similar distributed applications and we ended up find an almost perfect solution that worked out flawlessly for us.

[Back to main page!]({{ site.url }})
[Previous blog!]({{ page.previous.url }})
[Next blog!]({{ page.next.url }})

Last build time: {{ site.time }}
