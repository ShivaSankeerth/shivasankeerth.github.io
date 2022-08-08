---
layout: post
title: GSoC-First month analysis
tags: ['KDE','Google Summer of Code','GCompris']
---
As described in my previous blog post [GSoC Week 1](https://divyam3897.github.io/2017-06-10-GSoC-Week1-ComesToAnEnd/) I would 
be discussing about the **two player mode in Oware** in this blog post. This month was mainly spent in the initial design and the two player
mode of Oware. So here are the rules of the activity:

1. Each player has 6 houses in their possession with 4 seeds in each of the houses:  
![Oware board](/img/board.png)
You might have noticed a change, the buttons below to click the houses have vanished. We thought of saving that space for better
and also clicking on the houses directly is more intutive!!  

2. The players can click on any of the houses that they possess and the seeds in that house are than picked and 
dropped in the consecutive house in the process call **sowing**. A point to note here is if the player clicks on the house
having more than or equal to 12 seeds than while sowing the seed is not dropped in the house from where the player picked the seeds.  

3. While sowing if the total of seeds in any house of the opponent turn **equal to 2 or 3** then those seeds are **captured** by the player. 
The consecutive previous seeds are also checked, if their total count also reaches 2 or 3 then they are captured as well.  

4. However if all the houses of one player turn empty then the other player has to take such a turn that the opponent get some seeds
to continue the game.  

5. The player who captures 25 or greater seeds first wins the game :)

![Win stage](/img/win.png)

Animations are yet to be done where the seeds are seen moving in each turn to make it more interactive. Next in turn is the AI mode
where I will use Alpha beta pruning for making the computer give competition to the player :)
To play the two player mode you can see [Code Repository](https://cgit.kde.org/gcompris.git/tree/?h=gsoc_oware) . More about the AI mode in the
next blog post. :)
