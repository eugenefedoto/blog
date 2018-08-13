---
title: "Introduction to Scan Types (Part 1)"
date: 2018-08-03T11:32:38-05:00
draft: false
tags:
- Cheat Engine
- scan types
- Megaman 2
- NES
---

# A Little Background

I've always been interested in knowing how to hack. It's what got me interested in programming, and it started out with wanting to gain advantages in games. Though now I've become interested in further improving my knowledge and skills in computer science. With this series of hacking articles, I plan to write about my journey. I want to challenge myself in different areas (writing, programming, thinking, explaining), make this a hobby, and improve my writing skills.

These articles by me will focus on going into greater detail over concepts I've encountered in not so great detail and things I've had trouble piecing together due to lack of information from Google. For example, I take  Megaman 2 from Stephen Chapman's YouTube channel, then spend the majority of my time going over the concept of *memory mapping* instead of focusing on the simpler part of hacking the health value. Whereas Stephen did the reverse.

# Cheat Engine

Formal definition: Cheat Engine is a disassembler/debugger/memory editing GUI that works with little-endian* x86/x64 architectures.

*Partial big-endian support can be enabled with some hacks. An example of big-endian architecture is the Gamecube/Dolphin, which would cause CE's disassembler functions to be useless and values to be read in reverse.

Cheat Engine is a very popular intro into game hacking for many newbies. When the naive future-programmer becomes interested in games, he eventually stumbles upon cheats. It happened to me. One day, my friend introduced me to the Game Genie for the Sega Genesis. The world of assembly opened up to me. I knew nothing about it, besides seeing my friend input some magical codes. Later, when I found myself PC gaming, I learned about trainers and eventually stumbled upon that same magical world of assembly again. I played with it, but never seriously. It was a very confusing world with many tough concepts: pointers, hexadecimal, assembly code, machine code, injection, programming, etc. After graduating from computer science, I wanted to take a serious stab again at Cheat Engine. It's a simple looking program that is a great exercise in understanding the world of computers and strengthing my foundations.

# Setup

* Windows 10 x64
* FCEUX 2.2.3
* Megaman 2 (USA) for NES
* Cheat Engine 6.8.1 x64

# Before You Begin: Settings

<span style="color:red; font-size:20px">Important!</span> ![important][important]
```
Scan Settings -> check "MEM_MAPPED"
Debugger Options -> check "Use VEH Debugger"
```

## Explanation

Before I started finding the addresses, there were two settings I had to set in Cheat Engine.

1. Scan Settings -> check "MEM_MAPPED"
    * Memory Mapping -- a process in which regions of memory on one device are assigned address ranges from a shared device (peripheral) or file. A piece of hardware called a *mapper* on the game cartridge (or ROM) is used to map addresses from the cartridge to the reserved space on the console (or emulator). See this [Gamecube memory map].

    <figure>
        <img src="/~ef/assets/images/intro-scan-types/memmap1.gif"/>
        <figcaption>The file's (our Megaman 2 ROM) ranges are mapped into process 1 (the NES emulator). Also shown is shared memory between two processes, which is not used in our case.</figcaption>
    </figure>

    * CE doesn't detect mapped memory regions by default.
    * This is a setting you should always have enabled when working with emulators. It depends on how the emulator works and sometimes will work fine without it, but it's best to leave it on when dealing with any kind of emulator. In the below image, CE can't find the addresses we're looking for when the MEM_MAPPED setting is turned off and we try to find an address from that region. [![comparing mmap turned on and off]][comparing mmap turned on and off]
    * It's useless to enable for PC games (aka stored directly on PC memory) and will slow down scanning.
2. Debugger Options -> "Use VEH Debugger"
   * Some games have anti-debugger functions that crash the default Windows debugger.

# Hacking Megaman 2 (NES)

Megaman 2 is a simple game that is a good introduction to finding and changing values of simple properties such as health and energy.

1. Make sure you turned on the correct settings I explained above in [Before You Begin].

2. Run the NES emulator (FCEUX) and load the ROM (Megaman 2).

3. Attach the CE debugger to the emulator. *File -> Open Process (or use computer icon from the toolbar)*

4. Load any stage from the game. I chose Woodman.

5. Hack your health. Read below.

# Hack Your Health

Notice you start out with many bars for your health. Try to count them. Is it 30? No! The answer is 28, such a random number.

<span style="color:#57D900; font-size:20px">Tip!</span>![tip][tip]
```
Many times you won't need to know the value of something in order to find its address. 
This is a great scenario that illustrates the importance of never 
relying on values in front of your eyes.
```

Instead of counting, let's find it in a logical manner.

   1. Scan Type: Unknown Initial Value (great option to start within any game). Value Type and other settings are not important for this tutorial (will be later explained).

   2. First Scan -> I get 4.8 million addresses. You might have a completely different number by a few factors. This is fine.

   3. Go get hit by an enemy. Your hp reduces. Make sure to pause the game (press ENTER) while using CE so random enemies don't keep hitting you.

   4. *Scan Type: Decreased value* -> get hit again -> search ST: DV -> hit -> search ST: DV -> repeat ... You can also do *Scan Type: Increased Value* if you find a health pack. Also notice the red font, which indicates changing values from the last time CE polled the emulator. Tip: move around without changing the health and you can eliminate allow of those useless values using *Scan Type: Unchanged value*

   5. Eventually, you will end up with 1 address. Double-click it to add to your saved list at the bottom. Now feel free to change the value to whatever you like. Click the X under Active Column to enable God mode (aka *freezing* the value).

   6. Try gaining the maximum allowed health, or play around with CE and see which values gets you a full hp bar. It's 28!

# References

1. https://www.mathworks.com/help/matlab/import_export/overview-of-memory-mapping.html

2. https://smashboards.com/threads/using-cheat-engine-with-dolphin.442909/

3. https://medium.com/@fogleman/i-made-an-nes-emulator-here-s-what-i-learned-about-the-original-nintendo-2e078c9b28fe

4. [YouTube Cheat Engine Tutorial, Part 1 by Stephen Chapman]

[Gamecube memory map]: http://www.gc-forever.com/yagcd/chap4.html#sec4
[comparing mmap turned on and off]: /~ef/assets/images/intro-scan-types/mmap-on-off-dolphin.png
[Before You Begin]: #before-you-begin-settings
[YouTube Cheat Engine Tutorial, Part 1 by Stephen Chapman]: https://www.youtube.com/watch?v=hgrIKUR5Hww
[memory mapping between rom and emulator]: /~ef/assets/images/intro-scan-types/memmap1.gif
[important]: /~ef/assets/images/maki-chibi.png
[tip]: /~ef/assets/images/kotori-chibi.png