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

![It's Nova time](/assets/img/broughlike-intro-03-nova.gif)
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



[broughlike-intro-part-02]: /2021/04/15/broughlike-intro-02
[post-03-itch]: /
