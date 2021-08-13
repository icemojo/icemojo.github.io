---
layout: post
title: Broughlikes intro part 4 - The spikes
date: 2021-08-13
topics: 
- game-dev
- dev-log
---

# Spikes spell 

Ever since I've started thinking about spells system, I wanted to design something which would leave some sort of an object that persists in the game world. Maybe it could have some kind of a lifetime that would only last for a specific number of turns and be destroyed. Or something like a bomb that would explode after a certain number of turns countdown. But a bomb seems like a hybrid between the Nova and Leap landing splash damage, so I didn't do it. Bomb items that the player can hold separately would also makes sense, but the entire inventory system to hold those bombs could explode the scope tremendously. 

Then I turned into the idea of laying down traps for the enemy to step into. They would deal damage per turn, or even hold them in place for a specific number of turns. The latter idea sounds interesting but again I realized that it requires a lot of work to communicate how the trap behaves to the player. How many turns the trap will hold the enemy in place? Are they suppose to deal damage? What happens when all enemies are trapped? Et cetra, et cetra. 

I finally settled with a simple version of spikes that activates every other turn. When an enemy walks onto the tile with the spikes when it's activated, they'll get hurt. Only one spikes trap can occupy a tile. And since I don't want to limit the player's ability to cast the spells other than the gems, they can lay however many number of spikes as they pleased. But I added a small twist to prevent the player from spamming traps all over the map; the spikes can hurt the player too.

<p style="text-align:center;">
<img src="/assets/img/broughlike-intro-04-the-spikes.gif" alt="spikes" style="width:64%;">
<figcaption>Be weary of your own traps</figcaption>
</p>

As soon as the player cast the spell to lay the spikes under their feet, it actually starts deactivated. That gives you, as the player, a good opportunity to avoid it's damage initially. And if you're being chased by an enemy, they'll easily step onto it and get hurt on the next turn. Combined with the trap's two frames animation, I hope that's easy enough to communicate how it works and when it's dangerous. 


# Tentacles enemy 


# Totem Head enemy


# Other minor changes
