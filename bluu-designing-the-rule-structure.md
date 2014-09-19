```
title: Bluu: The Rules
tags: [Bluu, app, design, game]
date: August 1, 2014
slug: bluu-the-rules
excerpt: Creating the rule logic behind Bluu.
author: Andy Pai
```
### Bluu: The Rules
Designing the appropriate rule logic for Bluu was the was most time-consuming part of the weekend.  We wanted to create a game that took advantage of a psychological phenomenon known as the Stroop Effect, demonstrated below:

![img](https://dl.dropboxusercontent.com/u/2312024/StroopEffect.png)

The primary characteristics of any tile we could manipulate along with their abbreviations:

1. ITxC:  Inside Text Color
2. IBC:  Inside Background Color
3. ITx:  Inside Text
4. OTxC:  Outside Text Color
5. OBC:  Outside Background Color
6. OTx:  Outside Text

So the possible combinations were:

1. ITxC --> OTxC
2. ITxC --> OBC
3. ITxC --> OTx
4. IBC --> OTxC
5. IBC --> OBC
6. IBC --> OTx
7. ITx --> OTxC
8. ITx --> OBC
9. ITx --> OTx

### Attempt #1
#### Rule 1:  IBC  -->  OTx
Match the inside tile's background color (IBC) to the outside tile's text (OTx). To make it challenging, we ensured the text color of the outside tiles, never match the meaning of the text. For example, the user would have to match the center <span style="color:red;"> Tile </span> to<span style="color:blue;"> Red </span>listed on one of the four outer tiles.

#### Rule 2:  If IBC is Blue -->  OBC
If the inside tile is colored blue (IBC is blue), then match it to the outside tile's background color (OBC). Basically match inside<span style="color:blue;"> Tile </span> to outside <span style="color:blue;"> Tile</span>.

We realized this rule logic led to a lot of eye movement since you had to read the text on all four outside tiles at times.  So we decided it would be better if the center tile contained any text that required reading.

### Attempt #2:  Beta Testing Rules
#### Rule 1:  ITxC  -->  OBC
Match the inside tile's text color to the outside background color.

#### Rule 2:  If ITx is Blue -->  OBC
If the inside tile's text is blue, then match it to the outside tile's background color. Basically match inside '<span style="color:red;">Blue</span>' to outside <span style="color:blue;"> tile </span>.

#### Rule 3:  If TxC is Blue --> Anywhere except blue OBC
If the inside tile's text color is blue, then avoid matching it to the blue outside tile

We released a beta version with these rules, anticipating that Rule #3 may overly complicate the game. Beta testers agreed. 

### Attempt #3:  Final Rules for Launch
Rule 1:  ITxC  -->  OBC <br/>
Rule 2:  If ITx is Blue -->  OBC <br/>

If the game has app store success, it'll be fun remixing the rule combinations for challenge modes and additional levels!

In [Bluu: TestFlight vs. Crashlytics vs. HockeyApp](https://www.google.com) we describe our experience with selecting a beta testing and crash reporting service.
