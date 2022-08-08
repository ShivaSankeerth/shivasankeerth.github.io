---
layout: post
title: GSoC-Second month analysis
tags: ['KDE','Google Summer of Code','GCompris']
---

So, the second month came to an end, I wanted to write this blog post on my birthday :)
The second month started well, I made the initial version of AI using alpha beta but then
I was stuck at the animations. I tried various approaches, I started with the animations at
seed level and thought of moving all seeds together but in the initial stage I thought that
it's giving me less control as there were problems with accessing the seeds at the js level or ourside of the two nested repeaters.

Then I thought of moving it to house level, It started off well, the approach followed was that
I checked which house index the user has clicked example if the user has clicked on index 11 then
we move up and on the basis of the number of seeds I decided what should be next move but the problem here
was the same animation was called by all the strings, so basically yUp was called by all seeds as a result of
which the count of seeds became 0 at the very first move and the seeds didn't move further.

I discussed the situation with my mentors and was suggested to follow the same approach but at the seeds level.
Yes, you read it right, I turned to seeds level again. The approach was the same as house levels but I did it for each seed.
I started the initial animation for all the seeds at first:
```
/* If the indexValue on which player has clicked is between 6 and 11 then the first move will be towards right. */
if(items.indexValue >= 6 && items.indexValue < 11)
  nextMove = "right"
  /* Else if the indexValue on which player has clicked is 11 then first move will be up */
else if(items.indexValue == 11)
  nextMove = "up"
  /* Similarly if the indexValue on which player has clicked is between 0 and 5 then first move will be left and if equal to 0 then it will be down. */
else if(items.indexValue > 0 && items.indexValue <= 5)
  nextMove = "left"
else if(items.indexValue == 0)
  nextMove = "down"
  ```
  The nextMove here determines the next move of the particular seed on the basis of their current index which is maintained for each seed.

  On the basis of the current index I recursively called the startAnimation function for all the seeds:

  ```
  if(currentIndex >= 0 && currentSeeds > 0) {
    if(nextMove == "right" && currentIndex >= 0)
      xRightAnimation.start()
    else if(nextMove == "up" && currentIndex >= 0)
      yUpAnimation.start()
    else if(nextMove == "left")
      xLeftAnimation.start()
    else if(nextMove == "down" && currentIndex >= 0)
      yDownAnimation.start()
  }
```

That made them work, yes they are not complete yet but I am really happy to make them work. They took time but taught me alot.
I learnt alot of things in my second month. I agree the progress was probably a little slow but the animations were a whole new story
in themselves :)
I also learnt a few things which I would like to share:
1. Sometimes you need to just have patience. The animations looked like a big mountain for me when I started with them, even a single NumberAnimation didn't work
and to make my seeds work we had to try ScriptAction instead. It happened at times that I had no clue what I should do next but keeping patience helped me, I took a break from oware
for a day or two and ideas flowed through my mind in those days and I made it work when I got back :)
2. You should try to make life easy for people who test your work. I agree sometimes the things are required to be seen but well you should take your time instead and try to make
things as easy as possible for them to test, to save time and get better advice :)
3. Your commit messages describe your changes :) Why this? Well it happened that my commit message was not apt and resulted in a wrong perception/message. You should see what your commit is,
they describe your changes and should contain the changes in an apt and exact manner so that your mentors know what the changes are. It made me alot serious about my commit messages and
I tried to describe my changes in an apt way from thereafter :)

I also worked on note names in the breaks I took from oware and made the following changes to it:
1. Improved the instructions and overall design of the activity. Here is how it looks now:
![Note names](/img/noteNames.png)
2. Fixed the playing of notes on touch
3. Added note names in initial bass and trebel introduction.
![Introduction](/img/intro.png)

It has been a long post. I will work on fixing some oware issues and musical activities in this month. More about the changes in them in next post :)
