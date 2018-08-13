---
title: "Finding Problematic Values (Part 3)"
date: 2018-08-12T10:42:56-05:00
draft: false
tags:
- Cheat Engine
- values
- M.C. Kids
- NES
---

# Takeaway

Continue your understanding of not relying on in-your-face values.

# Challenges

* Fencepost error
* Finding illogical values through strategy

# Setup

* Windows 10 x64
* FCEUX 2.2.3
* M.C. Kids for NES
* Cheat Engine 6.8.1 x64

# Prerequisites

* Scanning simple values and types from parts 1 and 2.

# Hacking M.C. Kids

## Health (hearts)

Same as before:

* Scan Type: Unknown initial value; followed by Increased Value, Decreased Value, and Unchanged Value
* Value Type: All

<span style="color:#57D900; font-size:20px">Tip!</span>![tip]
```
You know, there are four hearts. There's a good chance that value is a byte.
Try it! However, also try the All type. I'll show you the problem with All type below.
I'm not saying there's a clear choice on which to use. Most of the time All type is great,
but sometimes making an educated guess can reduce error and pay off.
```

### Fencepost Error

<span style="color:red; font-size:20px">Important!</span> ![important]
```
You probably encountered problems finding the value. 
You said to yourself, "there are four hearts, so I'm looking for the value: {1, 2, 3, 4}."
That's wrong. You were actually off-by-one.
```
Like I said before in my articles, you can't trust what you're seeing. The game developers could've implemented the hearts in any way they pleased. The hearts could represent something crazy like {3423, 33434342, 1, 34242334}. It's possible, but unlikely. I'm just giving some crazy example. Something more likely would be {35, 36, 37, 38}. If you were paying attention to the values you were filtering down, you should've noticed the pattern {0, 1, 2, 3}, a fencepost error this time for you!

![fencepost-error]

<span style="color:red; font-size:20px">Important!</span> ![important]
```
Do you see those four addresses I circled?
They're the exact same address, except for the part after the colon: 
a byte, 2-byte, 4-byte, and double.
Remember how I said in the Tip above that you could've searched for Byte instead of All.
The Byte is the correct one, and the others don't make sense. 4 hearts is the max.
No point in going above a Byte nor whatever crazy value that double is.
```
## McDonald Arches (those M's)

You collect M's that persist through different levels. After reaching 100 M's in a level, you get a Bonus Game level after beating the current level.

* Scan Type: Unknown initial value; followed by Increased Value, Decreased Value, and Unchanged Value
* Value Type: Byte -- since we know 99 (resets to 0 when reaching 100) is the max value. Once again, All is always a great option, but this time Byte is a smart choice so we're going with that.

<span style="color:red; font-size:20px">Important!</span> ![important]
```
I bet you didn't find anything. WTF is going on. 
Time to step up your game plan! Continue reading.
```
You did everything correctly. You scanned -> ended up with a bunch of values not even close to your M score -> did one last scan and ended up with 0 results. Here's the deal: pay attention to the results before your last search (the one that gives you 0 results). 

<span style="color:#57D900; font-size:20px">Tip!</span>![tip]
```
You can press the Undo Scan button to get the previous values.
```

Here's my screen:

![arches]

Check out those two 42 and that 0 value byte. We're looking for patterns and those values seem the most logical. Not only are they the only single byte types, they're not a crazy number. So, try changing them all. What happens?

![arches2]

Both 42 are connected somehow with the 0 value, and they turn out to be not what we're looking for. But what happens when you change the 0? Modifying the 0 to something like 122, ends with changing our M score to 7M. Try other values such as 123 (a heart), 124, 130, 135, etc. I end up with 153 being M:99.

<span style="color:red; font-size:20px">Important!</span> ![important]
```
The developers decided to weave in some complex logic for figuring out the M score. 
Some numbers are symbols; some are numbers. 
It probably had to do with making use of the precious memory on the NES. 
Did you know the NES has 2KB of memory and this ROM has 256KB?
```

![arches4]

# Reference

1. [YouTube Cheat Engine Tutorial, Part 3 by Stephen Chapman]

[important]: /~ef/assets/images/maki-chibi.png
[tip]: /~ef/assets/images/kotori-chibi.png
[fencepost-error]: /~ef/assets/images/finding-problematic-values-(part-3)/health-values.JPG "Fencepost error: value is 0 at 1 heart"
[arches]: /~ef/assets/images/finding-problematic-values-(part-3)/arches-values.JPG "One of these is what we're looking for. Try to make an educated guess."
[arches2]: /~ef/assets/images/finding-problematic-values-(part-3)/arches-values-2.JPG "The original 0 value turns out to be what we're looking for. Also look at the 7M; the assets are sharing memory with the values we need."
[arches4]: /~ef/assets/images/finding-problematic-values-(part-3)/arches-values-4.JPG "153 is the value for M:99"
[YouTube Cheat Engine Tutorial, Part 3 by Stephen Chapman]: https://www.youtube.com/watch?v=5cKCPJcFYAU