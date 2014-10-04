```
title: Bluu: The Rules
tags: [Bluu, app, design, game]
date: August 1, 2014
slug: bluu-the-rules
excerpt: Creating the rule logic behind Bluu.
author: Andy Pai
```
If you didn't read my last post on [The Birth of Bluu](http://lapwinglabs.com/blog/the-birth-of-bluu), here's what you missed: we wanted to give ourselves a crash course in turning a new idea into a released app so that we'd have a better understanding of what it takes, and we figured a simple game would make for a good pilot project. This post talks about how we refined the idea for [Bluu](https://itunes.apple.com/us/app/bluu/id916926135?ls=1&mt=8) into something that we thought would be fun to play and easy to implement.

The idea behind Bluu was that we could take advantage of the [Stroop Effect](http://en.wikipedia.org/wiki/Stroop_effect) (depicted below) to create a game that was mentally stimulating but simple to implement.

![stroop effect](https://i.cloudup.com/6V0mxEDxpu.png)

But what would that game look like? How would you play it? Well, defining the actual rules for Bluu turned out to be the most time-consuming part of the weekend and required a few iterations.

## Keeping it simple
The goal of this project was to learn the intricacies of the overall app development lifecycle, not get bogged down in implementation effort, so we designed Bluu as a really simple 'race-the-clock' game that required players to quickly respond to 'text/color' challenges (whatever that meant...we hadn't really defined it at that point). 

To start, we laid out the gameboard of Bluu with a center ('Inside') tile surrounded by four 'Outside' tiles such that the goal of the game became to match the Inside tile with the correct Outside tile. Kind of an arbitrary decision, but we needed to start somewhere!

![img](https://dl.dropboxusercontent.com/u/13957782/Main.png)

This left us with the task of defining the rules for determining which 'Outside' tile would correctly match an 'Inside' tile.

## Matchmaker, Matchmaker...
The mockup above shows three primary characteristics of each tile that we could manipulate for each turn: Background Color, Word Color, and Word. This gave us a whole host of options for rule sets that would make two tiles 'matchable'. We tried a few different combinations, but they all followed the theme of doing things one way unless the color 'blue' was involved. This twist allowed us to add some difficulty to the game without significantly complicating the ruleset because it forces you to think critically at every turn.

### Attempt #1
#### Rule 1:  Match the background color of the inside tile to the correct word outside.
For example, in the mockup above you would flick the tile down, since its color is red and the bottom tile contains the word 'Red'.

#### Rule 2:  Unless the tile is blue!
If the inside tile is colored blue, then match it to the outside blue tile (so you're matching background colors in this case).

We realized this rule logic led to a lot of eye movement since you had to read the text on all four outside tiles at all times.  So we decided it would be better if *only* the center tile contained any text that required reading.

### Attempt #2
#### Rule 1:  Match the inside tile's word to the outside background color. 
So for example, if the inside tile contains the word "Yellow", you'd find the outer tile that was colored yellow and match with that. This required less eye movement and allowed us to speed up the game.

#### Rule 2:  Unless the tile is blue! 
This rule didn't change. If the inside tile is colored blue, then match it to the outside blue tile. 

We played the game internally with this ruleset, and while it was certainly an improvement over having to read 5 tiles per turn, we thought it became too easy after playing a few times. So we shuffled the rules around one more time before introducing to beta testers.

### Attempt #3:  Beta Testing Rules
#### Rule 1:  Match the inside tile's text color to the outside background color...
So if the word in the middle tile looked like <span style="color:red;">Green</span>, you'd send it to the outer tile whose background color was red. This added a little more complexity to the last revision of Rule 1 and more actively introduced the Stroop Effect by forcing you to acknowledge the color a word is printed in, even though it conflicts with the actual name presented.

#### Rule 2:  Unless the text is "blue"...
If the inside tile's text says blue, then you match it to the outside tile's background color. So '<span style="color:red;">Blue</span>' on the inside tile would match the outside tile colored blue. This rule keeps the game from getting too easy; without it you could just tune out the text entirely.

#### Rule 3:  Or you see <span style="color:blue;">blue</span>
If the inside tile's text *color* is <span style="color:blue;">blue</span>, then DO NOT match it to the blue outside tile! You can send it to any outer tile that's *not* colored blue. Again, this rule just adds a degree of difficulty and forces you to do some more complicated mental gymnastics before making the right match.

We released our beta version with these rules, anticipating that Rule #3 may overly-complicate the game. Our testers agreed, commenting that the rules were difficult to keep straight. We even got feedback from some saying that the game was broken because the rules were commonly misunderstood. So for launch, we stuck with only the first two rules from Attempt #3, dropping the 3rd for the sake of simplicity. We also added the tutorial cards that interactively teach you how to play and give you a chance to practice without time constraints so that you can test your understanding. You'll see these the first time you play the game.

If the game has app store success, it'll be fun remixing the rule combinations for challenge modes and additional levels!

In [Bluu: TestFlight vs. Crashlytics vs. HockeyApp](http://lapwinglabs.com/blog/bluu-testflight-crashlytics-hockeyapp) we describe our experience with selecting a beta testing and crash reporting service.
