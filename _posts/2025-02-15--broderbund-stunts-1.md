---
title:
layout: default
---

<style>
/* https://levelup.gitconnected.com/how-to-create-a-before-after-image-slider-with-css-and-js-a609d9ba77bf */
html {
	box-sizing: border-box;
}
*, *:before, *:after {
	box-sizing: inherit;
}

.container {
	position: relative;
	width: 640px;
	height: 480px;
	border: 2px solid white;
	margin-bottom: 1em;
}
.img {
	position: absolute;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
	background-size: 640px 100%;
}
.background-imgQ1Q {
	background-image: url('/assets/stunts/stunts--basic.png');
}
.foreground-imgQ1Q {
	background-image: url('/assets/stunts/stunts--supersight.png');
	width: 50%;
}

.background-imgQ2Q {
	background-image: url('si-setdesign-8x.png');
}
.foreground-imgQ2Q {
	background-image: url('si-setdesign-1x.png');
	width: 50%;
}

.slider {
	position: absolute;
	-webkit-appearance: none;
	appearance: none;
	width: 100%;
	height: 100%;
	background: #f2f2f200;
	outline: none;
	margin: 0;
	transition: all .2s;
	display: flex;
	justify-content: center;
	align-items: center;
}
.slider::-webkit-slider-thumb {
	-webkit-appearance: none;
	appearance: none;
	width: 6px;
	height: 480px;
	background: white;
	cursor: pointer;
}
.slider::-moz-range-thumb {
	width: 6px;
	height: 480px;
	background: white;
	cursor: pointer;
}

.slider-button {
	pointer-events: none;
	position: absolute;
	width: 30px;
	height: 30px;
	border-radius: 50%;
	background-color: white;
	left: calc(50% - 18px);
	top: calc(50% - 18px);
	display: flex;
	justify-content: center;
	align-items: center;
}

.slider-button::after {
	content: '';
	padding: 3px;
	display: inline-block;
	border: solid #5D5D5D;
	border-width: 0 2px 2px 0;
	transform: rotate(-45deg);
}
.slider-button::before {
	content: '';
	padding: 3px;
	display: inline-block;
	border: solid #5D5D5D;
	border-width: 0 2px 2px 0;
	transform: rotate(135deg);
}

</style>

# SuperSight: a graphical enhancement mod for Brøderbund's Stunts

You can get the binary in the [Downloads](projects/stunts.html) page. This and the following posts are instead meant to be a journal of this hacking project. Feel free to read it to learn some reverse engineering techniques or just to enjoy me ranting about the compiler.

But first, here's how the improvement looks like:

<center>
	<div class='container'>
		<div class='img background-imgQ1Q'></div>
		<div class='img foreground-imgQ1Q'></div>
		<input type="range" min="1" max="100" value="50" class="slider" name='sliderQ1Q' id="sliderQ1Q">
		<div class='slider-button' id="slider-buttonQ1Q"></div>
	</div>
	<figcaption>The start of ZCT282, vanilla game (left) and patched (right)</figcaption>
</center>

<script>
document.getElementById('sliderQ1Q').addEventListener('input', function(e) {
  const sliderPos = e.target.value;
  // Update the width of the foreground image
  document.querySelector('.foreground-imgQ1Q').style.width = `${sliderPos}%`;
  // Update the position of the slider button
  document.getElementById('slider-buttonQ1Q').style.left = `calc(${sliderPos}% - 18px)`;
});

document.getElementById('sliderQ1Q').addEventListener('change', function(e) {
  const sliderPos = e.target.value;
  // Update the width of the foreground image
  document.querySelector('.foreground-imgQ1Q').style.width = `${sliderPos}%`;
  // Update the position of the slider button
  document.getElementById('slider-buttonQ1Q').style.left = `calc(${sliderPos}% - 18px)`;
});
</script>

## What is this game?

Stunts (also known as 4D Sports Racing) is a racing game where the player controls real-life cars on very non-real-life tracks, packed full of jumps, loops, elevated roads and difficult terrain. If you are thinking of Trackmania (or Hard Drivin') you are on point: just add the charme of driving a Lamborghini or a Porsche instead of an anonymous vehicle. The most important difference, however, is a track editor that makes creating new circuits as easy as playing with Legos: it can really be picked up in a couple of minutes.

The fantastic editor is probably the reason why Stunts, even if it never attained cult status, still has a small but extremely active community. New content is continuously being created, as the track designers make use of 35 years of experience to enable incredible acrobatics, while other fans build new cars, aiming for maximum realism or for creative exploration of the possibilities offered by the game engine.

If you want to further explore Stunts's world, I'll include some further game-related resources in the appendix to this series. As a starter, I find it appropriate to link [kalpen.de](http://stunts.kalpen.de/allabout.htm), a site coming straight from the Web 1.0 era. Also, in this video you can see the game in the hands of one of the world's experts.

<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/Pwr3XwMWy_4?si=1GhQgpA2mO-t7PIr" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</center>

<!-- https://youtu.be/Pwr3XwMWy_4?si=8NblbaLi5684NzoR -->

## Starting to hack

You may remember that last year I created a very similar mod for a very similarly titled game, Stunt Island [article](/2024/11/20/tweaking-stunt-island.html). The analogies between the two games are many, which gave me hope to complete the task quickly by means of the same technique:
* attach a debugger to the game
* navigate to the options menu and change the graphics detail
* take note of where in the memory such variable is stored
* analyze the executable in a debugger and find where else in the code this memory location is being used
* one or more of such users must be code in the rendering algorithm
* alter the rendering code to increase the detail

Therefore, I thought the mod would take me a fraction of the time I needed for the previous project. This was an error. The first of a long series.

First thing, getting the executable. It comes in this format:
<center>
<a href="/assets/stunts/stunts--dolphin.png">
<img src="/assets/stunts/stunts--dolphin.png" alt="a 758-Byte .COM file" width="640"/>
</a>
<figcaption>That's 86 times smaller than <a href="https://www.pouet.net/prod.php?which=1221">.the .product</a>. Those programmers of yore knew their trade. </figcaption>
</center>

Well, there is no way that the game is contained there, so I must investigate further. Fortunately, during my preliminary research I found out that [stunts.hu], the biggest fan community for the game, contains a trove of information about the reverse engineering attempts of the game. The relevant bits must be pieced together by visiting the relevant subsections of [Wiki](https://wiki.stunts.hu/wiki/Category:Internals) and [forum](https://forum.stunts.hu/index.php?board=90.0), but it's a welcome alternative to the solitary effort that Stunt Island was. As [a 2009 post](https://forum.stunts.hu/index.php?topic=2454.msg43941;topicseen#msg43941) explains in detail, Stunts uses a multi-stage loading where `STUNTS.COM` calls `LOAD.EXE`, and `LOAD.EXE` decompresses and pieces together various binaries (depending on the graphic cars selected) to build up the real executable image in memory.

Luckily, I can ignore all these details because the community has already produced a much simpler `GAME.EXE` containing the final product. It can be grabbed from the Restunts repository, so I clone that and proceed to analyze the [executable](https://github.com/AlbertoMarnetto/restunts/blob/master/stunts/game.exe), blissfully ignoring what the rest of the repository has to offer. This is mistake #2.

I start `GAME.EXE` under [Dosbox-Debug](https://www.vogons.org/viewtopic.php?t=3944) and navigate to the options menu.

<center>
<a href="/assets/stunts/stunts--menu.png">
<img src="/assets/stunts/stunts--menu.png" alt="Stunts' options menu" width="500"/>
</a>
</center>

There I find out that, differently from Stunt Island, this game does not save its options in a file. This prevents me from setting a breakpoint on that event, as I did with Stunt Island. And just like it happened at that time, none of the other techniques (breaking on keyboard input, etc.) work. I open the executable in Ghidra to look up the strings in the menu, but they are not there.

I waste an hour there before deciding to have a look at what the rest of the Restunts package has to offer. And it's a lot. So much that, forget the options menu, this post will have to take a detour.

## The Restunts project

In the first decade of the century, some Stunts fans tried to get the source code of the game, to better understand the algorithms and create mods. [Kevin Pickell](http://www.scale18.com/cgi-bin/page/kpickell.html), one of the programmers, sadly informed them that the code was lost. As a consolation prize, it also told them that the game was to be considered freeware and so it could be distributed without problems. Undeterred, a handful of coders decided to reverse engineer the game from the machine code. So, not only they producted the synthesized `GAME.EXE`, but they fed it through IDA Pro which disassembled it and decomposed it in segments. After that, they started to label every assembly function and port them to a C equivalent. Although the energy faded out and led the program to a stop in 2015, they made [good progress](https://re.stunts.no/status/) on their analysis, leaving behind a lot of information.

A more detailed history of the project can be found on the relative [wikipage](https://wiki.stunts.hu/wiki/Restunts). But now, let's see what the contents are:

<center>
<a href="/assets/stunts/stunts--restunts-tree.png">
<img src="/assets/stunts/stunts--restunts-tree.png" alt="Restunts tree" width="223"/>
</a>
</center>

That's a lot! The `docs` contain notes about the game, `src` includes support utilities and side projects (like a full reconstruction of the MT32 driver), `stunts` contains `GAME.EXE` and an already build copy of the project (which should be equivalent to `GAME.EXE`, just with the code written in C) and `tools` provides the toolchain used to build the source, a strange collection of Windows and DOS tools, plus a Windows-based makefile system that glues together all the components.

The centerpiece, however, is obviously `src/restunts`, providing the game in two decomposed versions: `asmorig`, that contains the original disassembly coming from `GAME.EXE`, `asm`, a copy of it where the function ported to C are inactivated and replaced with calls to the C code, and `c`, the code of the ported functions.

For now, I do not want build the project; just reading the source code, enriched with years of annotations from the people working at this reverse engineering project before me, is a huge help. I could head directly to the graphic engine, but I'm set on the options menu, so I follow this thread of investigation.

## The options menu, revisited

Searching for `option` in the source code leads me to the ported `stuntsmainimpl` function, which is simple to understand.

<center>
<div style="display:flex; flex-direction: row; justify-content: center; align-items: center">
<a href="/assets/stunts/stunts--main-menu-code.png">
<img src="/assets/stunts/stunts--main-menu-code.png" width="400"/>
</a>
&nbsp; &nbsp; &nbsp;
<a href="/assets/stunts/stunts--main-menu-game.png">
<img src="/assets/stunts/stunts--main-menu-game.png" width="300"/>
</a>
</div>
<figcaption>It's very easy to find the section dedicated to the main menu in the core loop of the game. Its structure mirrors almost perfectly what the player sees.</figcaption>
</center>

Unfortunately, the function `run_option_menu` was not ported to C, so it's time to dive in the assembly code. With about 40 source files and no IDE to help me, `grep` is the workhorse. I find the function: it loads the menu text from a resource file which explains why I could not find the text in the executable. After a longish analysis, I isolate this fragment:

```
    call    show_dialog
    add     sp, 12h
    mov     [bp+var_2], al
    cbw
    sub     ax, 0FFFFh
    cmp     ax, 7
    jbe     short loc_13046
    jmp     loc_1315A
loc_13046:
    add     ax, ax
    xchg    ax, bx
    jmp     cs:off_1314A[bx]
```

So the function calls `show_dialog`, that (based on common sense and on what I see in other points) returns in the register `AL` the row number of the option chosen by the user. After some convolutions, the function uses the result for a computed jump in the table at `off_1314A`. I bet there was a `switch` statement in the original. I cannot understand some quirks (why adding 1? Why doing it with a `sub` instruction?) but it's not important.

<center>
<a href="/assets/stunts/stunts--options-menu.png">
<img src="/assets/stunts/stunts--options-menu.png" width="400"/>
</a>
<figcaption>This is less pleasant than before. </figcaption>
</center>

I follow the flow to `loc_13134`, which is a call to `do_mrl_textres`. I will rename the function in something more meaningful for future adventurers.

The function `do_mrl_textres` has about 200 lines of code, but the core is easy to find since the repeated comparisons stick out:

<center>
<a href="/assets/stunts/stunts--graphics-menu.png">
<img src="/assets/stunts/stunts--graphics-menu.png" width="400"/>
</a>
</center>

So, here my first discoveries: if one of the first 5 options is chosen, the result is stored in a program global, `timertestflag2`. I relabel this variable as `detail_level`, while `timertestflag`, thie option altered by choices 6 and 7, is renamed `slow_video_mgmt`.

<details>
  <summary>Aside: locating the assembly in DOSBox debug</summary>
  <p>
For added safety, and to find out the memory addresses of the flags, I want to set a breakpoint on the menu with DOSBox debug.

Finding where in memory the code is located, however, is a hard task. Stunts loads its routines in

 are stored in memory

https://shell-storm.org/online/Online-Assembler-and-Disassembler/
83 F8 05
3D 05 00

stunts--chatgpt-disassembly.png

<center>
<a href="/assets/stunts/stunts--dosbox-menu.png">
<img src="/assets/stunts/stunts--dosbox-menu.png" width="600"/>
</a>
<figcaption>This is less pleasant than before. </figcaption>
</center>
  </p>
## Aside

</details>
