---
layout: post
title: Broughlikes intro part 2 - The game
date: 2021-04-15 16:06:00 +0630
topics:
- game-dev
- dev-log
--- 

>This post is also available [on my itch.io project page][post02-itch].

# The map, movement and basic actions

Just like any other video game in existence, the moment to moment gameplay loop of moving the player around and, sometimes, attacking the enemies is the most important part in Broughlikes (or even Roguelike games in general). There's often a dilemma on whether killing monsters should be an integral part of the game, but I'm not actually quite experienced enough to discuss about it yet. 

The map generation is pretty much what you can expect from a game with simple two dimensional grids. There's a pre-defined chance on spawning solid/wall tiles on the map while it's being generated, and apply a [flood filling algorithm](https://en.wikipedia.org/wiki/Flood_fill) to the resulting grid to make sure there's no isolated, walled off areas. It's the size of the map where I faced the first hurdle with. The tutorial game was 7x7 with 1 extra tile for the boundary walls, which is just too big in my opinion. If the player was spawned on one corner of the map and the exit/goal is on the other corner, it takes forever to travel across the map without any enemies on the way. After spending some time tweaking the size and trying out a few wall spawning percentages, I ended up with 8x6 grid plus some additional space on the right for the HUD to fit everything nicely into a 16:9 screen ratio. Removing the boundary wall made everything so much cleaner too. It still kinda feel big from time to time, but for now, I'm just gonna roll with it.

Movement and actions are very much similar to what you can normally see in Broughlike games. Every movable entity moves tile by tile, and attack actions are carried out with the same inputs as the movement. So, if an enemy is blocking your path, you move into their tile to attack them, and they'll retaliate if they can, basically resulting in a brawl of endurance. The basic attacks in the tutorial game has this stunning effect on the enemies, which is absurdly powerful. In other words, if you can kite the enemies into a corner or a narrow corridor, you can essentially wipe out the entire map with the strongest of all enemies without losing any health. It would take a lot more work and content to balance out that simple behavior, so it has to go.

But I do like the idea of stun debuffs potentially being applied by a spell or an item later on, so when I was implementing the interaction between the player and the enemies, I did add an ability to apply stun effects. It's just that there's currently nothing triggering to stun either of the entities in any way. Without that stun effect on the basic attacks, you as a player are now more calculative with your actions. You can't mindlessly move into a tile close to an enemy without making sure that you can win the fight, or have a way to get out of it. That little simple mechanic is the basis of all Broughlike games.

## Dance maneuver

I hope you can see that the tactical moment-to-moment actions are the key to the Broughlike gameplay instead of brute force slaughtering through hordes of enemies. So, it is quite apparent that the player should be equipped with enough options to come up with the best possible move to survive in the next turn. There'll be explicit options such as casting a spell to heal yourself, or buffing/debuffing the entities on the map in one form or another. But the more subtle features I usually find very interesting, especially in Michael Brough's games, are through the enemy movements. 

Before I dive into that, I might need to disclose something about the enemy movements in this game. Their movements are, bottom-line, very dumb at this point in time. It's pretty much based on what the tutorial has explained (so I'm not gonna re-write everything in details anymore, because you can read up about it yourself anyway). The short version of it is, each enemy will see all four of their adjacent tiles (except the walls) and check which one of them seems to be closest to the player and move into it on the next turn until they're standing right next to the player themselves. I might be revamping that entire movement code using [A* algorithm](https://en.wikipedia.org/wiki/A*_search_algorithm) down the line, but dumb movement works for now.

But what I've added on top of it was, instead of just blindly identifying only one tile to move into, if there are more than one tile with the same distance towards the player, the enemy will randomly pick one of them based on 50-50 chance and then move into it. That simple improvement allows the player to seemly *dance* around an enemy if they want to prioritize their survival instead of directly engaging a superior enemy. 

![](/assets/img/broughlike-intro-02-dance-move-record01.gif)
<figcaption>You wanna dance, buddy? HAA... tricked ya!!</figcaption>

# Money and monsters

It's actually quite easy to spawn a number of shiny objects onto the game world, write a simple collision event handling code for the player and you've got a collectable system straightaway. The more challenging part is to figure out the actual purpose, the goal, the motivation of letting the player collect those shiny objects, rather than simply displaying an arbitrary amount of high score. 

Initially, I kinda cheaped out a little bit and made the level exit stairs to be enabled only when the player collected all the gems on the map. That apparently gave the game a stealth feel instead of a tactical dungeon crawler, because you're actively trying to avoid the enemies, focusing on getting all the gems and going down the stairs. In my own defense, I haven't actually figured out a proper loop for the enemies yet so there were no rewards for killing the enemies at this point in time. I immediately ripped it out and decided to turn the gems into a form of currency for casting the spells (like mana in standard RPGs).

<p style="text-align:center;">
<img src="/assets/img/broughlike-intro-02-shinies.png" style="width:60%;" />
<figcaption>Ohhh...... shinies!</figcaption>
</p>

Speaking of the enemies, there are also no proper distinctions between different enemy types at the moment too. They're currently just different sprites with different number of health points. Nothing more. There are three enemy types so far, and if the map were to randomly spawn a number of enemies after it's generated, it can't just entirely be random. It'd definitely be unfair for the player to face a Screaming Block enemy (the one with 6hp on the left) straight out of the bat on level 1. The randomness for the spawning system needs to be controlled and the difficulty needs to be smoothly spread out across a curve.

<p style="text-align:center;">
<img src="/assets/img/broughlike-intro-02-trapped.png" style="width:60%;" />
<figcaption>Trapped? ...or not!</figcaption>
</p>

The first instinct for someone would be to simply use a series of increasing or decreasing exponential values to control which enemies to spawn more or less at any given point in time. I've done it a couple of times before in the past, and the main problem with it was, the higher level the game goes the more difficult it is to predict where the game is on the difficulty curve without a giant spreadsheet opened on the side. Plus, expanding or adding more content to the system, like more enemy types, or adjusting the spawn rates for individual enemy types, is horribly painful. Instead, I came up with a nice little system that strikes a good balance between procedural and hand-made definitions. 

## Weighted spawning pools 

The Map object holds a reference to a series of spawn setting asset files (Unity scriptable objects) which contain a set of configurations for a pre-defined range of levels. In addition to the other dials, the prefab references and the 'Weight' values are the only two things I need to define for the individual elements under the 'Monster Pool' collection. After that I just need to hit the calculate button at the bottom, and the asset will automatically spread out the individual element percentage values between floating point 0 and 1. The Map's enemy spawning code can then use this to identify what needs to be spawned depending on a simple 0.0-1.0 randomized throw plus whatever level the player is currently in. 

![](/assets/img/broughlike-intro-02-weighted-spawning-pool-annotated.png)
<figcaption>An example weighted spawn setting between level 1 and 6</figcaption>

The example numbers shown in the screenshot might be a bit confusing at a glance. Just note that the 'Weight' value is a lot more flexible than a pre-defined percentage and you, as a designer, can use whatever value that makes sense for you. For instance, if you have 2 elements in the pool and you assign the weights of 3 and 1 each, then the percentages will be identified as 0.75 and 0.25 individually. And they'll be spread out as 0-0.75 and 0.76-1.0 respectively in the percentage ranges. On the other hand, if you only have one monster in the pool, whatever number you assign in the weight doesn't matter anymore because it'll be calculated as 100% spawning chance. 

This makes the spawning code WAY cleaner than I originally hoped for, because there is zero magic numbers hardcoded aside from the 0.0-1.0 randomization. Most of the grunt work was already done in the setting files. All it has to make sure is to cast the randomized floating point number into two decimal places in order to avoid weird precision problems that can always be seen in most modern programming languages. 

This system does have one biggest downside. You can't miss a single level in any of the level ranges when modifying the spawn setting files. You can see from the screenshot that the game currently has settings from level 1 to 18 at the moment. And if you accidently miss a level in any of those settings probably due to a small typo (let's say continuing from 8 onwards in the second setting instead of 7), then the enemy spawning code will just use the max setting of level 18 for level 7 because there is no definition for level 7. That is part of the edge case handling to make sure that the difficulty curve will simply becomes flat once there aren't any settings available for higher levels anymore. 

<p style="text-align:center;">
<img src="/assets/img/broughlike-intro-02-difficulty-curve.png" style="width:60%;">
<figcaption>Look at those currrvvvvves...........</figcaption>
</p>

The system is far from perfect of course, and I don't know if it's the right answer even for similar situations. There aren't any big drawbacks that can't be fixed with a little bit of refactoring. Plus, it's working nicely for this game at the moment with a lot of flexibility for expansion, so I'm quite happy with it. If I re-read this post in two or three years down the line, I may or may not see this solution as childish. But hey... that's one of the important things about dev logs, isn't it? Track your own journey of progression over time. 

# Leveling down and leveling up

With the difficulty curve established, there's nothing much to write home about advancing down into the dungeon level by level, floor by floor. The current plan is to use the number of floors/levels the player were able to reach in a single game session as a form of high score. That's about it. There's one slight problem with this plan though. The player can potentially ignore everything else the game has to offer (the combat, collectable gems and the spells) and focus on just getting down as much as they can. I don't really know how to deal with it yet to be honest. One idea is to block off the progression every few levels or so, probably with a boss or something, but I don't really know how it's gonna feel. Plus, I don't have enough interesting regular enemies yet to even start thinking about bosses, so consider this a free cheat code for high score chasers. 

The player will get healed 1 health point when going down a level though. That's mainly to subtly encourage the player (with a small reward) that it's ok to just simply move on if they weren't able to kill every single enemy or collect every gem in a level if they're not quite healthy enough. 

I mentioned above that there isn't any special characteristics to each individual enemies at the moment. But they'll grant the player some experience points (specifically, 1) when they're killed. When the player's accumulated enough experience points, a random spell will be unlocked in the hotbar, and the amount of points they'll need to reach the next level (gain another spell) will be increased slightly. You know... just a standard but simplified RPG leveling mechanism. 

<p style="text-align:center;">
<img src="/assets/img/broughlike-intro-02-experience.png" style="width:60%;">
<figcaption>With experience, comes wisdom.</figcaption>
</p>

# Spell system

You can probably see now that all the elements we have so far are now serving as a good foundation for a proper game loop. 

- Kill enemies => gain xp => acquire random spells => kill more enemies (or) survive longer 
- Collect gems => cast spells => kill more enemies (or) survive longer 
- Survive longer => descend more into the dungeon => get higher score 

Not the most innovative in the video game industry, but it works fine. 

One of the most common elements in Broughlike games (or maybe even turn based games in general) is the *wait* command. There will not be a dedicated wait command (or a spell) in any form, because it's already embedded within the spell system itself. In other words, casting a spell will cost you a turn and you can tactically use this to bring the enemies into more favorable positions if you play smart. That brings me to the topic of assigning the gem costs for the spell. 

Now that each individual spells are doing at least two things, the cost would be slightly higher than it should normally be. The **Heal** spell will heal the player with 1 hp and lets you wait for a turn, so it costs **2 gems**. The **Blink** spell will teleport the player to a random unoccupied tile on the map, and lets you wait for a turn, but it only costs **1**, which means the value of the Blink spell is non-existent. Because of it's random nature, it's actually a bit of a gamble to blink out of a tight situation. You might get lucky, or you might get trapped and ended up close to an enemy with only 1 hp left. 

The third spell, **Cannibalize** took a little bit more work to design. It costs **2 gems** but it has a special requirement to be presented on the map to cast; *corpses*. Corpses (or as it is currently being rendered in the game, *blood puddles*) were originally just aesthetics that mark the spot where a monster died. Then, when I started working on the spell system, I figured I could do something with them. Initially, I started out as healing the player when they cast the spell while standing on a corpse. And since multiple corpses can stack on one tile (if multiple enemies died on the same spot) the healing effect increases based on the number of corpses on that tile. With a few play tests, it was immediately apparent that the spell doesn't feel interesting at all, and also obviously redundant with **Heal**, which has the same cost but can be cast anywhere on the map. 

<p style="text-align:center;">
<img src="/assets/img/broughlike-intro-02-cannibalize-record02.gif" style="width:60%;">
<figcaption>Take THAT!</figcaption>
</p>

So I changed the Cannibalize to *a temporary damage boost* which lasts only for 1 turn. In other words, if you've just cast the spell while standing on a corpse, you better use it straightaway. This makes it a lot more interesting than giving health points to the player. Because, in a map with two regular enemies spawned, if you've managed to kill the first enemy, you can make a quick work of the second one with the Cannibalize damage boost. But you'll have to time it and position yourself quite right not to waste it. 

# Halfway through the journey 

Like I've mentioned in [the first post][post01], I've already got about sixty hours of work into this project so far and I'm having a lot of fun with it. So I'm gonna keep working on it until I hit a hundred hours mark (and maybe even a little bit more) before wrapping up the project. This soft target is essentially there to prevent me from scope creeping and also giving myself a permission to stop working on the project once I've passed that target hours.

So, I'll see you again in a few weeks. 

[post01]: /2021/04/10/broughlike-intro-01
[post02-itch]: https://icemojo.itch.io/7drl2021/devlog/242923/broughlikes-intro-part-2-the-game
