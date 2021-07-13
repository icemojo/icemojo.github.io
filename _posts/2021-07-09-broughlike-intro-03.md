---
layout: post
title: Broughlikes intro part 3 - The leap
date: 2021-07-09
topics: 
- game-dev
- dev-log
--- 

It's been a while since I've last talked about this project, so much so that people might assume I've already abandoned it. Quite contrary actually, I'm still very much knee deep into development. The game was supposed to be wrapped up within fourty more hours or so after the [last post][broughlike-intro-part-02], which is essentially about two weeks of work. The reason why that's not happening yet is a lot of things have been going on in my life and I couldn't get much dev hours as I would've wanted. Important life event, failed game jams and difficult job hunting experiences occupied my time a lot over the last two months. Now that I've just wrapped up one of the big features a couple of days ago, I figured it's as good as time as any to check in before I proceed. 

So, first of all...

# It's getting dark, baby!

![Darker color scheme](/assets/img/broughlike-intro-03-darker-color-scheme.png)

Yup, the entire color scheme of the dungeon has been changed into a darker tone. The main reason was, well, it's supposed to be a dungeon filled with scary monsters and shifting floors. The previous yellowy color scheme just feels like you're strolling through an ancient, faraway, desert temple. Which works fine by itself, but the vibe I wanted to go for is quite similar to Diablo 1 or 2, where you just couldn't know what dangers or riches await on every descent. 

Plus, the player is now a green viking dude holding an axe, which will probably never get used physically in the entire game. The previous blue alien-like guy with a pot belly is now revamped into an enemy monster with it's own special ability. We'll talk more about him in a minute, but for now, let's dive into the new spells. 

# Nova spell

I've been thinking about adding a range offensive spell of some sort for a while. Creating a spell which shoots out a projectile in one cardinal direction is easy to implement for sure. But at the same time, I don't really want to undermine the main mechanic of the game, which is getting close to the enemies in order to bring them down while considering about your positioning on the map every single turn. As a balancing act, I can make that projectile spell expensive to cast in order to discourage the player from using it frequently. But, that would probably make the game feel punishing just to do such a simple thing you can always see everywhere else. 

![It's Nova time](/assets/img/broughlike-intro-03-the-nova.gif)
<figcaption>It's Nova time</figcaption>

So, the solution becomes the beams (or waves) of energy that radiate outwards into all four cardinal directions of the player. It costs 3 gems to cast, won't go through the walls and hit everything in the straight lines with 1 damage. With enough gems in your pocket, you as the player can still lay waste to multiple smaller enemies if you can position yourself quite right. But you're still vulnerable to incoming attacks if they can get close enough to you since casting the spell advances the turn. 

I thought about adjusting the gem cost a little bit. But you can probably get only one to two enemies, maybe three at most in one shot. Plus, with a low damage rate of 1, you won't be able to overpower tougher enemies in the late game without the help of other spells either. So, 3 gems works for now.

# Leap spell

Now, here comes the primary focus of this post, and also one of the reasons why very little progress was made during this past couple of months. I wanted to include a spell which allows the player to target a walkable/passable tile, jump onto it and damages all surrounding enemies upon landing there. There's an emphasis on *jump* here by the way. Quoting Diablo again, I have to acknowledge that I'm closely imitating one of the Barbarian's skills with the similar name of course. I can simply lerp (linear interpolate) the player from one position to another and call it a jump. But, no no no! I've already done it with Blink. That's not what I want for Leap. 

![Diablo Leap attack](/assets/img/broughlike-intro-03-diablo-leap-attack.jpg)
<figcaption>Diablo 2 Barbarian's Leap Attack -- a classic move</figcaption>

In a game where moment to moment actions are more focused rather than fast movements, adding a spell which can allow you to traverse a long distance can sound a bit contradictory. Consider it as a form of top tier/ultimate ability, where you're moving and damaging enemies at the same time. Whichever the case, I just wanted to implement it and see how it feels in the game.

Besides, I've never done a parabolic arc movement in games before and I've always wanted to do it some form or another. So, it'll be a good learning opportunity for me as well. I mean, Angry Birds has this, most of the RTS and tower defense games have this, pretty much all the games which have a form of archery have this. How hard could this be, right? 

Well, spoiler alert! It's VERY hard. 

![Example games with arc traversal](/assets/img/broughlike-intro-03-arc-examples.png)
<figcaption>From left to right, top to bottom; Angry Birds, Tank Wars, Tower Fall, Legends of Kingdom Rush</figcaption>

Naturally, I started researching the basic physics principles behind it, like [projectile motion](https://en.wikipedia.org/wiki/Projectile_motion) and [kinematic equations](https://en.wikipedia.org/wiki/Equations_of_motion). Most of the theories didn't make much sense to me without a proper context to apply them. I mean, I think I understand them, but most of the time, I just feel like I'm not smart enough to implement them on my own. So I did what most modern laymans would do, turn to YouTube and forums for direct guidance on how to properly implement parabolic projectile motion in games. There's obviously no shortage of resources to consume of course. [Sebastian Lague's videos on kinematic equations](https://www.youtube.com/playlist?list=PLFt_AvWsXl0eMryeweK7gc9T04lJCIg_W) are pretty good. (I'm a constant follower of his work.) [This guy made a small game](https://www.youtube.com/watch?v=7ZYkCOmF0yc) about artic foxes with pouncing (jumping) as a main mechanic. There are tons of tutorials, explanations and code examples from realistically shooting an arrow to predicting the trajectory of a projectile. Even so, most of them didn't found to be useful for me either. 

One of the main thing I wasn't really satisfied with most of these tutorials (not all of them, but most) was, they either rely too much on a specific game engine and its physics components without explaning very much about the math behind it, or just straightaway showcase the working code that implemented the proper motion equations and give no explanation at all. Most of them were like; "if you want to shoot a projectile, apply some initial velocity to it like this and let physics engine take care of the rest." or "if you want to find out where the projectile would land after applying a certain velocity in a certain angle, plug in those formula and you're good to go." I even found a video which literally explained and coded just like another video. The most common theme is of course to treat the object in motion as a projectile (a cannon ball, a rocket, an arrow, or whatever), which means they focus on calculating the initial velocity and angle of the object and have very little control once it's in motion. That's not exactly what I wanted either. 

Then I slowly realized that there *are* actually different ways to implement the parabolic arc motion. They're all based on one basic displacement formula. But, depending on what you want to achieve, you're gonna have to think about which values to use as inputs and how to derive the unknown values into independent x and y components. 

<p style="text-align:center;">
<img src="/assets/img/broughlike-intro-03-displacement.jpg" style="width:50%;" />
<figcaption>The good ol' displacement formula we've never paid attention to in high school</figcaption>
</p>

So, I stepped back a little bit and think about what I want the object (rather, the player) to behave, and also allows me to control different aspects of the entire traversal movement. 

- I know the start and end positions of the object very well. 
- I want to control the angle of the trajectory, which of course should be higher up from the target position. Otherwise, it'll just be shooting in a straight line. 

These sound trivial. But what I wanted to do depending on these inputs is to calculate various *displacement values* of the object across the traversal arc over time, so that;

1. I'll be able to render the full trajectory arc line properly in order to give visual feedback of how the object is going to move, 
1. and control the movement of the object across the arc at any given point in time just by sampling one of those same displacement values. 

![traversal arc concept](/assets/img/broughlike-intro-03-traversal-arc-concept.jpg)
<figcaption>The objective is not to throw an object to a target, but to control the entire traversal movement over the parabolic arc</figcaption>

 I know it's a lot of rambling and much of it won't make sense without additional explanations with proper mathematical formula. I still don't fully understand much of it even after I've managed to make it work. I hope that references you've found in this article would give you a good runway when you're trying to attempt your own implemenation. All I want to say is that it's not just about throwing an object from one point to another. And you should try to lay out concrete intentions, either as a designer or a programmer, and do a thorough research before diving into futher implementation details. 

So after about 18 hours of various prototype iterations over a couple of weeks, here's what I got. I'm not gonna go through all of my prototypes here just to save your sanity. But I do want to note that [this video by Gingerageous Games](https://www.youtube.com/watch?v=F47dmKpAIW0) brought me closest to what I wanted to achieve. 

![traversal arc implemented](/assets/img/broughlike-intro-03-traversal-arc-implemented.gif)
<figcaption>Smooth jazz moves</figcaption>

Yes, as you can probably guess, control over the movement is a big part of my requirement. Notice the displacement values array in the Inspector is re-evaluated in real time? All I have to do to move the object across the arc is to interpolate between those coordinates. It doesn't matter whether it's moving forward or backward. And the same array is used for both movement and rendering the trajectory line as well.

The final solution is far from perfect, and there were a few kinks I had to additionally work out when putting everything together in the game. But it works well enough, and also allows me to design everything in a nice modular way so that the same exact logic could be easily reused aside from the Leap spell. 

Let's time jump a dozen more dev hours and check out how it turned out.

![the leap](/assets/img/broughlike-intro-03-the-leap.gif)
<figcaption>Leaping in and out of actions</figcaption>

I have to say that the spell looks kinda cool in action. Just wanna note down a couple of things here. 

A keyboard controlled *reticle pointer* is used for targeting the spell. Other than the fact that it took several hours just to make the reticle work as smooth as it is, I'm not gonna dive deeper into it anymore. (This article is already lenghty as it is.) Though you might be wondering why I chose keyboard to control the reticle instead of the mouse on a PC game. It might be so much easier and precise to control the spell, right? 

Well, yes, it might. The thing is, there's no other mechanic or feature in the game that requires mouse input. The movement is controlled by keyboard, the spells hotbar has numerical bindings in each slot, action confirmation is with space bar. Even the menu navigation will be through keyboard. So it'll be quite jarring for the player to reach out to the mouse just to cast this single spell. Trust me, I know it is. Because I've seen a few jam games done this, and it really throws me off guard. 

The spell costs 5 gems and deals 1 damage to surrounding enemies upon landing at the target tile. You can reach anywhere on the entire map with it. The only limitation is directly onto the walls or the enemies. It really feels like it has a good offensive and utility purposes, doesn't it. The high cost would, hopefully, prevent the player from overusing it. 



[broughlike-intro-part-02]: /2021/04/15/broughlike-intro-02
[post-03-itch]: /
