---
title: Disassembling Stunt Island
layout: default
---

## Tweaking Stunt Island's 30-year-old 3D engine

Note: the patch is available HERE. This post is about its making-of.

### Intro

Stunt Island is a flight simulator from the year 1992. You can read more about it in my [previous post]({% link _posts/2024-11-15--stunt-island-elegy.md %}). 

Last month I finally beat the game, winning all the missions and getting the title of “Stuntman of the year”. While I was replaying the videos of my attempts, as well as some fan-made SI films in the channel, I wondered if I could somehow mitigate the obvious limitations of the rendering engine.

Also, I was always fascinated by the art of reverse engineering, and I thought this could be a good opportunity to learn something in this field. Assembly is not new to me, but dealing with an archaic DOS program, working without gdb's features and any kind of debug symbols means entering an entirely different world.

I spent a dozen of hours learning the tools of the trade and trying to figure out the puzzle, and I was very happy of my eventual success. After finding and patching the right location in the executable, the engine got some extra horsepower. No extra resolution unfortunately, but many more details, as one can see in the comparisons below.

In the following, I'll show my process and some failed tries. Even if this is far from being a professional effort, I hope that publishing this can be of help to other learners, or maybe induce some experts to chime in and suggest improvements (in the name of Cunningham's Law). Tutorials about debugging with DOSBox are rare to find, so I feel that even this small example can be useful.

Also, I hope to revive some interest in an old, but beautiful and unique game.

### Start

Since I have never debugged DOS programs, the first step is to build up the toolset. I need

* DOSBox debug to run and live-debug the game
* Ghidra to decompile and analyze the executable. There are other DOS-compatible decompilers around, but I liked Ghidra “late Nineties” UI, even if it does not render nicely on Linux. I used the older 10.4 version for compatibility with my Debian's openjdk package.
* Bless, my favorite hex editor

I start by opening the game executable in Ghidra. The first result is underwhelming:

<center><img src="/assets/2024-11-20--stunt-island-disassembly/ghidra-raw.png" alt="drawing" width="800" border="1"/></center>

Just one function and a lot of non-decompiled bytes, that does not look good. Maybe the decompile process failed?

Let's look at the function. I cannot grasp all the details, but it seems to be copying some data from X to Y. This rings a bell – could it be that this stub is self-decrypting the rest of the program?

Also, what does the file header look like? 

```
$ hexdump -C STUNT.EXE | head -5 
00000000  4d 5a 18 00 0d 01 00 00  02 00 20 1c ff ff e9 3c  |MZ........ ....<|
00000010  80 00 00 00 0e 00 52 21  1c 00 00 00 4c 5a 39 31  |......R!....LZ91|
00000020  ff 3f e8 64 00 a3 12 62  89 16 14 62 e8 81 00 3b  |.?.d...b...b...;|
00000030  3e ec f9 75 04 3b 06 ef  76 1b ea fd c7 ff 0f 06  |>..u.;..v.......|
00000040  1c 62 00 00 b4 43 cd 67  84 e4 75 37 4f 87 ee 16  |.b...C.g..u7O...|
```

Seeing the “LZ91” finally clicks for me. This game was probably compressed by some variant of the Lempel-Ziv algorithm. Some googling leads me first to [this article by Sam Russell](https://www.lodsb.com/reversing-lz91-from-commander-keen) and then to [the UNLZEXE unpacker](https://moddingwiki.shikadi.net/wiki/UNLZEXE). Turns out that the original compression tool was authored by the prolific genius Fabrice Bellard. 

OK, let's decompress the executable and reload it in Ghidra:

<center><img src="/assets/2024-11-20--stunt-island-disassembly/ghidra-si-uncompressed.png" alt="drawing" width="600" border="1"/></center>

Looks better. Can we make any sense of the functions?

<center><img src="/assets/2024-11-20--stunt-island-disassembly/ghidra-complex.png" alt="drawing" width="600" border="1"/></center>

Nope, this is over my paygrade. But we are in 2024, maybe we can seek the help of a LLM? Copilot says:

<div><small><blockquote>
This looks like assembly code mixed with some higher-level C code constructs, dealing with system-level operations. Here’s a bit of a breakdown:<br/>
* Initial Setup: The function begins by initializing variables and handling some immediate checks and adjustments.<br/>
* Comparisons and Bit Manipulations: It checks some global variables and performs bit shifts and comparisons to adjust the control flow based on these conditions.<br/>
* System Calls: There are several system interrupt calls, likely interacting with hardware or the operating system.<br/>
* Function Calls: The code calls other functions (like FUN_1f59_36f7, FUN_1f59_0bc2, and FUN_1f59_0c2c) which perform some specific tasks.<br/>
* Loops: The code includes loops and conditional checks to perform repeated tasks or to continue execution based on certain conditions.<br/>
* The main flow is about handling and manipulating certain global variables and possibly hardware states, based on the names and types used. However, without more context on these global variables and the overall program architecture, it's tough to pin down the exact purpose.
</blockquote>
</small></div>

Not really useful. Thanks all the same, AI. See you in some years. Let's go back to carbon-based intelligence.

### A matter of detail

The static analysis did not yield any result yet, but I have another idea. Stunt Island has a “Detail” setting: 

<center><img src="/assets/2024-11-20--stunt-island-disassembly/si-detail.png" alt="drawing" width="640" border="1"/></center>

My hope is that the 100% value is stored in some variable, and that we can maybe change it to 255, or see how the computation is performed.

Let's fire up Dosbox debug. The best manual for the interface is [a thread on the Vogons forum](https://www.vogons.org/viewtopic.php?t=3944). 

<center><img src="/assets/2024-11-20--stunt-island-disassembly/dosbox-debug.png" alt="drawing" width="600"/></center>

The GUI is nice, even I miss the amenities of modern debugging, especially the function names. However, credit is due to the authors: they put in nice features like a branch predictor (shows whether a jump instruction will be taken with the current status of registries and memory, and if so, whether we will jump back or forward).

We must somehow find out where in the memory the value for the detail is stored. I navigate again to the “Preferences” screen and spend a lot of time there, trying to devise a method to find out where the program stores the “detail” value. My list of failed attempts includes:
* In Ghidra, look for a printf format string with a number and a percent (something like `"%u%%"`): nothing found
* Break on keyboard input: I set a breakpoint on keypress (`BPINT 16`) and press “Return” with the mouse pointing on the “plus” sign. The program breaks in a input-reading routine, but stepping through the code I soon become unable to understand what the code is doing. Considering that there could be a long routine dedicated to the UI drawings, I soon give up.
* Break on mouse move or click: there is a [helpful guide](https://www.vogons.org/viewtopic.php?f=31&t=83267) for that, but again I end up in a mysterious land.
* Break on pixel change: I manage to break the program when a pixel in the detail scroller changes (`bpmem a1000:3500`). The code is not easier than before.

Looking for ideas, I find a nice [list of techniques](https://wiki.scummvm.org/index.php/HOWTO-Reverse_Engineering) by the makers of ScummVM. One of the options is “File Access”, which reminds me that the game must store its preferences somewhere. A quick search by date reveals that the file `GAME.VAR` is changed every time I alter the settings. Let's see how this file changes by setting a different detail level. For 99% we have

```
$ hexdump -C GAME.VAR 
00000000  70 fd ff 01 ff ff ff ff  ff ff 99 e7 0b 00 38 01  |p.............8.|
00000010  64 02 0b 00 57 01 64 02  00 00 00 00 00 00 00 00  |d...W.d.........|
00000020  00 00 00                                          |...|
00000023
```

Now let's set 1% detail:


```
$ hexdump -C GAME.VAR 
00000000  8f 02 ff 01 ff ff ff ff  ff ff 99 e7 0b 00 38 01  |..............8.|
00000010  64 02 0b 00 57 01 64 02  00 00 00 00 00 00 00 00  |d...W.d.........|
00000020  00 00 00                                          |...|
00000023
```

So the detail level is in the very first two bytes. Unluckily, it seems already to be scaled, with 100% = 0xffff. No chance to simply set it higher. But let's not lose hope so soon: we can find out where in the memory this value is stored and see if something else in the calculation can be altered.

We set a breakpoint on file write. I use an [old but practical](https://mrszeto.net/CIT/interrupts.htm) guide to the interrupt table to find out that a file write is triggered by calling interrupt 21h with 40h in registry AH. Let's set a breakpoint there: 

<center><img src="/assets/2024-11-20--stunt-island-disassembly/si-bpint-21-40.png" alt="drawing" width="800"/></center>

Change the detail level, close the preferences window and, bingo! Breakpoint triggered.

<center><img src="/assets/2024-11-20--stunt-island-disassembly/si-config-save.png" alt="drawing" width="800"/></center>

The bytes to write are in DS:DX, so 1628:2FEA. Let's have a look:

<center><img src="/assets/2024-11-20--stunt-island-disassembly/si-2fea.png" alt="drawing" width="800"/></center>

Yes, the uint16 at that address matches the detail level we set (`0x3d70/0xffff ~= 0.24`). So we now know that the detail level is there. I bet that this is a global variable and will stay there for the whole duration of the program.

<center><img src="/assets/2024-11-20--stunt-island-disassembly/si-detail-global.png" alt="drawing" width="800"/></center>

It does. Very nice.

### Following the flow

Now we have to see where this variable is used. Let's go back to Ghidra, and look for the low two bytes.

<center><img src="/assets/2024-11-20--stunt-island-disassembly/ghidra-2fea.png" alt="drawing" width="800"/></center>

We are lucky, not too many places.

First place: CS:4CDA. Ghidra decompiles the code to

```
    *(int *)0xb86 =
         (int)((ulong)*(uint *)0x2fea * 100 >> 0x10) + *(int *)0xa5a +
         (uint)((int)((ulong)*(uint *)0x2fea * 100) < 0);
```

Not super clear, but the meaning becomes evident when I set a breakpoint there (`BP CS:4CD8`): the line is executed only when I open the preferences box. So the “100” in the code must be part of the rescaling to a percentage.

Second place: CS:680E. Better to look at the assembly:

```
       1000:6809 a1 ea 91        MOV        AX,[0x91ea]
       1000:680c f7 26 ea 2f     MUL        word ptr [0x2fea]
       1000:6810 d1 e0           SHL        AX,0x1
       1000:6812 d1 d2           RCL        DX,0x1
       1000:6814 89 16 f2 91     MOV        word ptr [0x91f2],DX
```

So, the variable at DS:91EA get multiplied times the detail level, and then written at DS:91F2. No idea what DS:91EA is, though. I first think that this routine is calculating some per-polygon quantity, but it turns out that it is only run once per frame. Also, when I run some frames in mission 1 the variable at DS:91EA switches between 0x0999 and 0x7FFF. Let's ignore the matter: we now know that at DS:91F2 something gets written that is proportional to the detail level. Moreover, with Detail set at 100% this variable is often set at 0xFFFD, almost UINT16_MAX.

We look for `f2 91` in Ghidra and it only yields two occurrences. One is the fragment we just analyzed, the other is in the middle of some bytes that Ghidra did not decompile automatically. 

<center><img src="/assets/2024-11-20--stunt-island-disassembly/ghidra-91f2.png" alt="drawing" width="800"/></center>

Well, there is no other place to look, so we ask Ghidra to decompile this section. Some attempts later, a plausible code appears:

<center><img src="/assets/2024-11-20--stunt-island-disassembly/ghidra-91f2-found.png" alt="drawing" width="800"/></center>

We confirm that we got the decompilation right by setting a breakpoint at the start of the fragment (`BP CS:76E8`), and for added safety we verify the program counter (EIP) really points to the numbers shown in the listing. This code gets executed many, many times per frame – have we finally reached the goal?

### Ace gets glasses

Let's try to understand the code: first it decrements AX and executes what follows only if it's still positive. Not sure what this means but some live debugging shows that the condition is met quite often anyway. The following calculation is easier to understand: in the x86 architecture, calling MUL multiplies AX times the operand, leaving the most significant 16 bits of the result in DX, the low ones in AX. With maximum detail, we know that DS:91F2 will be set at 0xFFFD, so that the multiplication is almost a 16-bit left shift, that sets DX to (AX_old - 1). The lower the detail, the lower DX will be. Then the code executes some code only if `CX <= DX + BX`, otherwise it gets skipped.

We know from Sanglard's interview that each 3D entity in the game is represented as tree, and the engine decides whether to render the (coarser) top nodes or the (finer) lower levels according to their distance from the observer. And we see that some pointer game gets executed if CX is under a certain threshold, which gets bigger the bigger is the detail level. Let's make the bold hypothesis that CX is the object distance, and (DX + BX) determines the distance threshold under which a higher-resolution model gets loaded.

We can test the hypothesis quite easily: we just overwrite the calculation with new code, which just sets the threshold to, say, zero. Ghidra shows us which bytes to insert:

<center><img src="/assets/2024-11-20--stunt-island-disassembly/ghidra-patch.png" alt="drawing" width="800"/></center>

We can do that in the live program: 

<center><img src="/assets/2024-11-20--stunt-island-disassembly/dd-patch-live.png" alt="drawing" width="800"/></center>

The thing works perfectly: all details disappear, Jackson City is reduced to flatland.

<center><img src="/assets/2024-11-20--stunt-island-disassembly/flatland.png" alt="drawing" width="640"/></center>

But what we want is the contrary: setting the threshold so that the max resolution gets used for all objects. Let's just set it to the max:

```
SM cs:76f4 c7 c2 ff 7f 66 90 
(MOV DX, 0x7FFF ; NOP) 
```

This makes the engine totally unresponsive to the keyboard commands. I guess I was too greedy. Let's be more modest:

```
SM cs:76f4 c7 c2 ff 4f 66 90 
(MOV DX, 0x4FFF ; NOP) 
```

And here we are: Stunt Island in the best detail you'll ever get in a 320x200 window! Pity that the system is still just barely responsive: I cannot control my plane, and this is the best screenshot I am able to take before crashing.

<center><img src="/assets/2024-11-20--stunt-island-disassembly/sr71-high-detail.png" alt="drawing" width="640"/></center>

But why bother? The rendering routine is the same for flight, video player and set designer, and at least in the last mode we can take all the time we need to steer the camera. And here we are: Jackson City and surroundings, more gorgeous than ever!

<center><img src="/assets/2024-11-20--stunt-island-disassembly/set-des-high-detail.png" alt="drawing" width="640"/></center>

Now that we explored the limits, let's turn back the dial and try to find some compromise to make the game playable. The way I patched the code was brutal, I just used a fixed threshold ignoring the original value in DX. We know that DX is approximately `AX_old * (detail_level/100)`, maxing out at `AX_old - 1` if the detail level is 100%. What if we instead put a multiple of `AX` there? The space is limited, but a shift instruction can fit. After some experiments, I settle on

```
SM cs:76f4 89 c2 c1 e2 03 90  
(MOV DX,AX ; SHL DX,0x3 ; NOP)
```

which would be the equivalent of a 800% detail.  

And I can finally celebrate: the quality is great and the game (after cranking up Dosbox to the max) runs still fluidly. Time to celebrate!

<center><img src="/assets/2024-11-20--stunt-island-disassembly/final-quality.png" alt="drawing" width="640"/></center>

By the way, the fact that we are discarding the most significant three bits of DX does not seem to lead to any noticeable glitch; we could add some code checking for overflow and saturating the value, but this would require more bytes than we are replacing. We might create the algorithm elsewhere and then JMP to it, but at the price of making the code slower and the patching process more difficult.

### Washing the dishes

After enjoying a Stunt Island looking better than ever, I still have some homework to do.




mouse: https://www.vogons.org/viewtopic.php?f=31&t=83267
https://mrszeto.net/CIT/interrupts.htm


bpmem a1000:3500 break on detail scroller
ds:4fea : var for detail
cs:4cd8 : multiply detail times 100 for representation
cs:680c : calculate scene? 1 volta x frame
ds:91f2 : some threshold? referred at cs:76f4
cs:76f4 called by cs:75af
1f59:06bb some geometrical calculation?

rewrote at 76f4:
SM cs:76f4 f7 26 f2 91 03 d3  original
SM cs:76f4 c7 c2 ff ff 66 90  no detail
SM cs:76f4 c7 c2 ff 7f 66 90  too much detail?
SM cs:76f4 c7 c2 ff 4f 66 90 YEAH!!!
SM cs:76f4 c7 c2 00 00 66 90 strange detail

patched at 0000:7af4: c7 c2 ff 4f 66 90
(i.e. MOV DX, 0xffff; nop)
patched at 0000:7af4: 89 c2 c1 e2 03 90
(i.e. MOV        DX,AX; SHL        DX,0x3 ; NOP)

PLAYONE: look for f7 26 till found a matching construct : found @ 212df in code, 252d in file
f7 26 a4 22 03 d3
89 c2 c1 e2 03 90

 found @ 1f5b in file
f7 26 a4 22 03 d3
89 c2 c1 e2 03 90

MAKEONE-v0
found @256c
F7 26 76 22 03 D3
89 c2 c1 e2 03 90

MAKEONE-v3
found @267A
F7 26 FE 1A 03 D3
89 c2 c1 e2 03 90

string to search
03 d3 3b ca 7f 1b 8b 6e 08 8b 5e 00 


For stunt island patch3
have to patch unlzexe and remove the version detection
search for 03 d3 3b ca 7f 1b 8b 6e 08  8b 5e 00 
replace 4 bytes above: 0x7dda




