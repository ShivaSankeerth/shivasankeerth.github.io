---
layout: post
title: Note names
tags: ['KDE','Google Summer of Code','GCompris']
---

I mentioned in my [previous blog](https://divyam3897.github.io/2017-08-03-GSoC-Month2/) that I started with the note names activity.
This will be a musical blog covering the different components that we have and some music knowledge :)

I had been a fond of music from playing a piano to a guitar, that being a reason 
for me working on [background music](https://cgit.kde.org/gcompris.git/log/?h=backgroundMusic) and making the musical activities as a part
of my GSoC. 
Music is generally represented with Staff. So what is a Staff? The Staff
consists of 5 horizontal lines on which our musical notes lie. We represent the 
lower pitches lower on the Staff and the higher pitches are represented higher on the
staff. 
```
Repeater {
  model: nbLines
  Rectangle {
    width: staff.width
    height: 5
    border.width: 5
    color: "black"
    x: 0
    y: index * verticalDistanceBetweenLines
  }
}
```

nbLines = number of horizontal lines = 5  

But with blank staff can you tell what notes will be played? No, we can't, we use Clef
for that. We have two main Clefs: Base Clef and Treble Clef. Also more notes can be added
to a staff using Ledger lines which are used for extending the Staff. We can specify the type of clef
using which the notes are represented.
```
Repeater {
  id: staves
  model: nbStaves
  Staff {
      id: staff
      clef: multipleStaff.clef
      height: (multipleStaff.height - distanceBetweenStaff * (nbStaves - 1)) / nbStaves
      width: multipleStaff.width
      y: index * (height + distanceBetweenStaff)
      lastPartition: index == nbStaves - 1
      firstNoteX: multipleStaff.firstNoteX
    }
}
```

We can even have multiple staffs by specifying the nbStaves in MultipleStaff component. For note names we have nbStaves = 1 whereas clef = treble for levels < 10 whereas
cleff = bass for level > 10
```
MultipleStaff {
  id: staff
  nbStaves: 1
  clef: bar.level <= 10 ? "treble" : "bass"
  height: background.height / 4
  width: bar.level == 1 || bar.level == 11 ? background.width * 0.8 : background.width / 2
  nbMaxNotesPerStaff: bar.level == 1 || bar.level == 11 ? 8 : 1
  firstNoteX: bar.level == 1 || bar.level == 11 ? width / 5 : width / 2
}
```

I did various changes and fixes in the last week in note names which include:
1. Adding highlighting to options in the levels.  
![Highlight of notes](/img/highlight.png)
2. Fixing keyboard controls which allows you to navigate between options using arrow keys and selecting
the answer using Enter or return key
3. Added the initial version of highlighting of the notes on staff for note names.

In the coming days, I will work on the following things:
1. Improving the highlight of notes on staff.
2. Add a drag for the options in levels.
3. Cleaning the code and other minor fixes :)

Did I tell you that I am also working on more animations for oware? Yes, we have more animations for oware coming up for the movements of the seeds when captured to the score houses.
I completed the animation movement pretty quickly this time as compared to the time taken when I was implementing it for the movements. Probably that was due to all
the things I learnt in those animations which made me realise that though it took a lot of time (made me behind my timeline alot :D) but it was totally worth it. At the end
our aim is to provide the best activities for kids with the best experience that they can get and not just workable activities. Along with clean and maintainable code
to make it as easy as we can for new contributors or anyone to understand. Well, that's what you learn the most :)

I will share more about the note names activity and the score animation also in my next blog post :)
