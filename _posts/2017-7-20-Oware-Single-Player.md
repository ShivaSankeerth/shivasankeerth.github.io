---
layout: post
title: Oware - Single player
tags: ['KDE','Google Summer of Code','GCompris']
---

I completed the Oware two player mode as described in my previous blog post 
[GSoC-First month analysis](https://divyam3897.github.io/2017-07-01-GSoC-Month1/). 
This month was the time for the single player mode. So well it started well,
I implemented the alpha beta pruning for the AI mode.

The levels are structured in the following way:
1. For the initial levels the AI is random which sows randomly from its houses. But if 
the computer gets in a bad position or is losing badly then it switches to AI mode.
A **maxDiff** is maintained for each level, for example level 1 has maxDiff = 20,
level 2 has maxDiff = 15 and so on. So as soon as the computer/AI is behind the player
by maxDiff it switches to AI mode.
2. For the last levels there is no random selection so basically maxDiff = 0.

I also had been working on the animation of seeds which involves movement of the seeds
from one house to another. It still has some issues left involving the animation being triggered
at each step. 

There are also some changes in my GSoC plan. I thought of working on computer activity after oware 
earlier when I made my proposal but with discussions with my mentors we came to a conclusion that
musical activities are more important for a child. So I would be working on musical activities now 
which includes play piano and note names activity which were started in the branch [Play piano](https://cgit.kde.org/gcompris.git/log/?h=playpiano)
and were earlier to be done in my last month. My aim would be to complete both these activities.

I planned to go to Akademy this week and meet everyone but well my visa got refused :( I will try to be a part 
next year and I hope I get the chance of meeting everyone (especially my mentors soon). I will cover more about the musical activities and my progress in my next blog post :)
