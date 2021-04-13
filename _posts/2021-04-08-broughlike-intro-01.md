---
layout: post
title: Broughlikes intro part 1 - The discovery
date: 2021-04-08 15:24:00 +0630
topics:
- game-dev
- dev-log
---

About a month ago, I stumbled upon [this nice little tutorial series](https://nluqo.github.io/broughlike-tutorial/), which plays on a very niche sub-genre of the Roguelike games called "Broughlikes". For those of you who don't know what "Roguelike" means, I'll trust your ability to Google stuffs in order to respect the time of those who do. But if you're not really into Roguelike games at all, I suppose this article will mostly just bore you. 

So, the term Broughlike was named after an indie developer, Michael Brough, who created a series of charming, weird games like [Imbroglio](https://apps.apple.com/nz/app/imbroglio/id969264934) and [868-Hack](https://apps.apple.com/us/app/868-hack/id635749911). Imbroglio has actually been one of my favorite and longest standing game on my phone ever since I saw [this Gamasutra article](https://www.gamasutra.com/view/news/273314/Freedom_through_constraints_The_design_of_Michael_Broughs_Imbroglio.php) a few years ago. Back then, even though I was immediately hooked with the game after I bought it, I only took it as a small Roguelike game with interesting puzzle elements. Nothing more. That is, of courese, until I discovered the tutorial series mentioned above. 

There are [a few key characteristics to Broughlike](https://itch.io/jam/broughlike) games that make them stand out even from regular Roguelikes. But the main premises that stuck around in my head for so long (ever since I've started playing Imbroglio) were the concept of “small worlds”, very refined and controlled randomness, and simple yet very deeply designed game rules. There are a couple of other games that came into my mind which has similar properties to it ([Road Not Taken](https://store.steampowered.com/app/293740/Road_Not_Taken/) being one of them, [Crypt of the Necrodancer](https://store.steampowered.com/app/247080/Crypt_of_the_NecroDancer/) being another) but they mostly focus on the idea of Roguelikes rather than just smallish, confined worlds. I’ve always wanted to play with this design concept myself too, but never been able to get into it. Mostly because I didn’t know where to start, how to design the mechanics or how things simply just fit together in a tiny space. The tutorial finally gave me a good jump start.

THIS is essentially what I ended up with after working furiously on the tutorial for about 20 hours.

![](/assets/img/broughlike-intro-01-tutorial.png)
<figcaption>Killer background, I know</figcaption>

I’m not including this screenshot for showing off my ability to follow a step-by-step tutorial. Instead, I’m using this as a reference point before I move onto a proper project and put my own spin on it. Because if you play the game that comes with the tutorial, you can immediately get a feel of how janky it is. The gameplay is incredibly inbalanced, your own basic attack is downright overpowered, a lot of the elements like the spells and enemy behaviors are confusing as hell. I’m not bad-mouthing the tutorial series of course. The series itself was very well done. And the game that came out of it was objectively never meant to be a good one either. I’m only mentioning this just to get my own reference point for how much I’ve improved myself on approaching this design concept in the coming weeks. Like I said before, I didn't really know where to start. Now that I do, if I can't make something at least a little bit better than this, then I haven't learned anything at all.

# Enter... the 7 Days Roguelike Challenge

After I wrapped up the tutorial game, I was so hyped with what I've learned, I immediately wanted to start working on something of my own. And what better way to apply the new found knowledge than working in a game jam? I didn't have to look around much, because sure enough, the [7DRL Challenge 2021](https://itch.io/jam/7drl-challenge-2021) was currently underway. Even though the jam was a week long, I only caught the tail end of it with less than 72 hours to work with. Well, I've been in multiple 48 hours jams before, so how hard could it be to finish something within that period of time. Spoiler alert, I didn't finish in time for submission. One of the main reason was me wasting one third of that available time picking out THE perfect game engine to work with for this particular type of game (which I DEFINITELY don't normally do in similar scenarios ;-], but that's gonna be a story for another time).

BUT... rather than simply demoralized and abandoned the game starightaway (like I ALSO definitely don't usually do), I kept working on it. So, this article or rather, this dev log series, is mostly about my experience working on a little bit longer form project than usual, lessons learned and maybe talk about some of the design decisions I made along the way. And since I've already got almost 60 hours of work under my belt, I figured this is a good time as any to pause for a moment and reflect a little bit. 

So, here goes. 

![](/assets/img/broughlike-intro-01-after-sixty-hours.png)
<figcaption>Current progress after 60 hours of iterative works</figcaption>

Even though I'm using the tutorial game as a starting point, I didn't actually reuse its code base at all. Instead, I've started everything from scratch in Unity (aside from upscaling the sprites I've already drawn while working on the tutorial of course). I came very, very close towards building my own engine using a framework like MonoGame or something similar as a baseline, because basic functionalities like the map generation, movement, entity relations and hierarchy are that simple and specific enough to not warrant a reliance on any of those shiny features available in modern game engines. If you can draw a 2D sprite and move things around on the screen relatively easy, then you're pretty much good to go. But for now, let's just say that learning how to design interesting mechanics is my primary goal for this project. 

## The map, movement and basic actions

Just like any other video game in existence, the moment to moment gameplay loop of moving the player around and, sometimes, attacking the enemies is the most important part in Broughlikes (or even Roguelike games in general). There's often a dilemma on whether killing monsters should be an integral part of the game, but I'm not actually quite experienced enough to discuss about it yet. 

The map generation is pretty much what you can expect from a game with simple two dimensional grids. There's a pre-defined chance on spawning solid/wall tiles on the map while it's being generated, and apply a [flood filling algorithm](https://en.wikipedia.org/wiki/Flood_fill) to the resulting grid to make sure there's no isolated, walled off areas. It's the size of the map where I faced the first hurdle with. The tutorial game was 7x7 with 1 extra tile for the boundary walls, which is just too big in my opinion. If the player was spawned on one corner of the map and the exit/goal is on the other corner, it takes forever to travel across the map without any enemies on the way. After spending some time tweaking the size and trying out a few wall spawning percentages, I ended up with 8x6 grid plus some additional space on the right for the HUD to fit everything nicely into a 16:9 screen ratio. Removing the boundary wall made everything so much cleaner too. It still kinda feel big from time to time, but for now, I'm just gonna roll with it.

Movement and actions are very much similar to what you can normally see in Broughlike games. Every movable entity moves tile by tile, and attack actions are carried out with the same inputs as the movement. So, if an enemy is blocking your path, you move into their tile to attack them, and they'll retaliate if they can, basically resulting in a brawl of endurance. The basic attacks in the tutorial game has this stunning effect on the enemies, which is absurdly powerful. In other words, if you can kite the enemies into a corner or a narrow corridor, you can essentialy wipe out the entire map with the strongest of all enemies without losing any health. It would take a lot more work and content to balance out that simple behavior, so it has to go.

But I do like the idea of stun debuffs potentially being applied by a spell or an item later on, so when I was implementing the monsters interactions, I did add an ability to apply stun effects. It's just that there's currently nothing triggering to stun the monsters in any way. Without that stun effect on the basic attacks, you as a player are now more calculative with your actions. You can't mindlessly move into a tile close to an enemy without making sure that you can win the fight, or have a way to get out of it. That little simple mechanic is the basis of all Broughlike games. (A side object-orientation note here; when I say 'monster' at any given point in time, I'm referring to the parent class of both the player and the enemy entities since they both share a lot of properties and behaviors.)

## Money and monsters

It's actually quite easy to spawn a number of shiny objects onto the game world, write a simple collision event handling code for the player and you've got a collectable system straightaway. The more challenging part is to figure out the actual purpose, the goal, the motivation of letting the player collect those shiny objects, rather than simply displaying an arbitrary amount of high score. 

Initially, I kinda cheaped out a little bit and made the level exit stairs to be enabled only when the player collected all the gems on the map. That apparently gave the game a stealth feel instead of a tactical dungeon crawler, because you're actively trying to avoid the enemies, focusing on getting all the gems and going down the stairs. In my own defence, I haven't actually figured out a proper loop for the enemies yet so there were no rewards for killing the enemies at this point in time. I immediately ripped it out and decided to turn the gems into a form of currency for casting the spells (like mana in standard RPGs).

<p style="text-align:center;">
<img src="/assets/img/broughlike-intro-01-shinies.png" style="width:60%;" />
<figcaption>Ohhh...... shinies!</figcaption>
</p>

Speaking of the enemies, there are also no proper distinctions between different enemy types at the moment too. They're currently just different sprites with different number of health points. Nothing more. There are three enemy types so far, and if the map were to randomly spawn a number of enemies after it's generated, it can't just entirely be random. It'd definitely be unfair for the player to face a Screaming Block enemy (the one with 6hp on the left) straight out of the bat on level 1. The randomness for the spawning system needs to be controlled and the difficulty needs to be smoothly spread out across a curve.

<p style="text-align:center;">
<img src="/assets/img/broughlike-intro-01-trapped.png" style="width:60%;" />
<figcaption>Trapped? ...or not!</figcaption>
</p>

The first instinct for someone would be to simply use a series of increasing or decreasing exponential values to control which enemies to spawn more or less at any given point in time. I've done it a couple of times before in the past, and the main problem with it was, the higher level the game goes the more difficult it is to predict where the game is on the difficulty curve without a giant spreadsheet opened on the side. Plus, expanding or adding more content to the system, like more enemy types, or adjusting the spawn rates for individual enemy types, is horribly painful. Instead, I came up with a nice little system that strikes a good balance between procedural and hand-made definitions. 

### Weighted spawning pools 

The Map object holds a reference to a series of spawn setting asset files (Unity scriptable objects) which contain a set of configurations for a pre-defined range of levels. In addition to the other dials, the prefab references and the 'Weight' values are the only two things I need to define for the individual elements under the 'Monster Pool' collection. After that I just need to hit the calculate button at the bottom, and the asset will automatically spread out the individual element percentage values between floating point 0 and 1. The Map's enemy spawning code can then use this to identify what needs to be spawned depending on a simple 0.0-1.0 randomized throw plus whatever level the player is currently in. 

![](/assets/img/broughlike-intro-01-weighted-spawning-pool-annotated.png)
<figcaption>An example weighted spawn setting between level 1 and 6</figcaption>

The example numbers shown in the screenshot might be a bit confusing at a glance. Just note that the 'Weight' value is a lot more flexible than a pre-defined percentage and you, as a designer, can use whatever value that makes sense for you. For instance, if you have 2 elements in the pool and you assign the weights of 3 and 1 each, then the percentages will be identified as 0.75 and 0.25 individually. And they'll be spread out as 0-0.75 and 0.76-1.0 respectively in the percentage ranges. On the other hand, if you only have one monster in the pool, whatever number you assign in the weight doesn't matter anymore because it'll be calculated as 100% spawning chance. 

This makes the spawning code WAY cleaner than I originally hoped for, because there is zero magic numbers hardcoded aside from the 0.0-1.0 randomization. Most of the grunt work was already done in the setting files. All it has to make sure is to cast the randomized floating point number into two decimal places in order to avoid weird precision problems that can always be seen in most modern programming language. 

This system does have a few downsides obviously. The biggest one being;

- You can't miss a single level in any of the level ranges. You can see from the screenshot that the game currently has settings from level 1 to 18 at the moment. And if you accidently miss a level in any of those settings probably due to a small typo (let's say continuing from 8 onwards in the second setting instead of 7), then the enemy spawning code will just use the max setting of level 18 for level 7 because there is no definition for level 7. That is part of the edge case handling to make sure that the difficulty curve will simply becomes flat once there aren't any settings available for higher levels anymore. 

<p style="text-align:center;">
<img src="/assets/img/broughlike-intro-01-difficulty-curve.png" style="width:60%;">
<figcaption>Look at those currrvvvvves...........</figcaption>
</p>

- The map spawning code and the spawn setting files are not quite independent from each other even though there's zero need for hardcoded numbers. For instance, the setting definition works on the assumption that the whatever code it's being used from, they'll work under the range of 0.0 and 1.0 when identifying the percentage. Nothing a few code refactorings can't solve, but it's worth mentioning as well. 

The system is far from perfect of course, and I don't know if it's the right answer even for similar situations. But, it's working nicely for this game at the moment with a lot of flexibility for expansion, so I'm quite happy with it. If I re-read this post in two or three years down the line, I may or may not see this solution as childish. But hey... that's one of the important things about dev logs, isn't it? Track your own progression over time. 

## Leveling up and leveling down


## Spell system
