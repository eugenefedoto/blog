---
title: "2 - Importance of Value Types"
date: 2018-08-04T00:46:25-05:00
draft: false
tags:
- Cheat Engine
- value types
- Ninja Gaiden 2
---

# Setup

* Windows 10 x64
* FCEUX 2.2.3
* Ninja Gaiden 2 for NES
* Cheat Engine 6.8.1

# Prerequisite

* Knowledge of scanning simple values. See: [Intro Scan Types]

# The Value Type

Before, I only paid attention to the *Scan Type*, where I said it's always best to leave it at *Unknown initial value.* No need to know anything about assembly or how bytes are stored. The next step is *Value Type*, which is important and needs some explanation. Memory is similar to hard disks; we use both to store possibly anything, only limited by the capacity of the RAM or HDD, and by how much we care about speed. Right now, we don't care about speed. We care about 32 bit vs 64 bit. Your computer supports one or both (most likely both), which means it can store 2^64 bits = 2^64 different addresses = 2^64 values. The maximum length of a value is 64 bits so your RAM will be storing either a byte, 2 bytes, 4 bytes (32 bits), or 8 bytes (64 bits) in a single location, and these are the scan options CE provides us with.

Your goal is to find the scan type that is a perfect match.

# Explanation

The game developer decides to store all character data as 2-byte values inside the game because the game is a complex space shooter and a byte would only allow us to use values 0-255. We try and search for the shield number, expressed as the value 1,500. 1,500 won't fit inside the byte (1,500 > 255). We can use a 2-byte. 2^16 - 1 == 65,535. If there's a chance for the shields to go above 65,535, we'd have to switch to 4-bytes.

What if we initially choose 4-bytes for a value that's actually a byte? That's the problem we encounter with Ninja Gaiden 2. You wrongly tell CE to search for 4-bytes. What happens if CE naively finds the health byte you're looking for and ends up reading an extra byte or two because you gave it the wrong setting?

![wrong scan type]

Usually, there will be trash data in that extra byte which will give you the wrong results. Take 0xAC31 for example. 31 could be the health we're searching and AC would be the trash byte. If by chance the extra byte is empty, like the AC in 0xAC31, you could still get the correct result you were looking for.

<span style="color:#57D900; font-size:20px">Tip!</span>![tip][tip]
```
It can be troublesome to figure out the correct *scan type*. You can waste your energy
individually trying the byte, 2-bytes, and 4-bytes. There's a much better way.
Use the All type. See "Important!"
```

<span style="color:red; font-size:20px">Important!</span> ![important][important]
```
Settings -> Scan Settings -> All type includes (check all)
```

# Hacking Ninja Gaiden 2 (NES)

Same as Megaman 2.

1. Turn on everything like you did before. Correct settings and all as explained previously.

2. Make sure *All* is enabled, as explained by Kotori's tip above.

3. *Scan Type: Unknown initial value; Value Type: All*

4. Easily find the health value. This time the max health is 16.

# Reference

1. [YouTube Cheat Engine Tutorial, Part 2 by Stephen Chapman]

[YouTube Cheat Engine Tutorial, Part 2 by Stephen Chapman]: https://www.youtube.com/watch?v=v-5wehWM3tE
[Intro Scan Types]: /~ef/intro-scan-types
[important]: /~ef/assets/images/maki-chibi.png
[tip]: /~ef/assets/images/kotori-chibi.png
[wrong scan type]: /~ef/assets/images/importance-value-types/health-4bytes.JPG "got a trash value due to not searching for Byte"