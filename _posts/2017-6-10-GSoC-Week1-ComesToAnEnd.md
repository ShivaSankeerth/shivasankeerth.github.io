---
layout: post
title: GSoC 2017, Week 1 comes to an end
tags: ['KDE']
---

So the week 1 comes to an end. The week was not normal as all weeks. It
took off good but then had something really bad instore for me. Yes, something
really bad. So no more of a suspense, the bad thing was my laptop which was working
fine in the sunny morning was not turning on in the afternoon. You heard it 
right, I lost my laptop amidst the first week :( I took it for repair and it 
was expected to return in 3-4 days.  

I had to find a workaround. I then thought of setting up GCompris in my college.
I worked, made patches everyday and sent them to my mentors to commit them for me. Thankfully
they were really supportive and helped me during that phase. As a result of 
which I accomplished my work for the first week:
1. I implemented the basic layout for the activity which involves setting up the 
score item, the oware board etc.  
![Basic Layout](/img/layout.png)  
2. I made a generic component for the tutorial for an activity which looks like:
![Tutorial](/img/tut1.png)  

A few lines of code are required to get the tutorial section now.
```
Tutorial {
    id: tutorialSection
    tutorialDetals: Activity.tutorialInstructions
    onSkipPressed: {
        Activity.initLevel()
    }
}
```
where the tutorialInstructions contains the instructions and images for each instruction for
the respective activity.

My plan for the next week is to implement the two player mode for oware. Luckily my laptop
has also returned now and I am back to work on it now. You can find the code at [GSoC_oware](https://cgit.kde.org/gcompris.git/log/?h=gsoc_oware) More about the two players mode in the upcoming week :)


