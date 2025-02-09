---
title: "Stunts Modification Projects"
layout: default
permalink: /projects/stunts.html
---

This page contains my mods of DSI's Stunts (aka 4D Sports Driving). I'll be discussing the changes (under my username HerrNove) in [ZakStunts' forum](https://forum.stunts.hu/index.php?board=90.0).

The code for these projects is available in [this repo](https://github.com/AlbertoMarnetto/restunts).

## Supersight

A mod increasing the field of view to a radius of up to 10 tiles around the car and drawing all objects in high resolution. For very crowded scenes, it automatically reduces the detail to avoid running out of memory. Press F5 to see if this is happening (it will print the number of discarded tiles in the upper-left corner). See [my post](https://forum.stunts.hu/index.php?topic=4400.msg96441#msg96441) for more details – I intend to produce a more detailed writeup soon.

Download:
* <b>[Supersight v1.4](/assets/stunts/RESTUNTS-747817b.EXE)</b>

A demonstration video:
<video controls width="640">
  <source src="/assets/stunts/stunts-282ARG-3.mp4" />
  <!-- ffmpeg -i *.mkv(om[1]) -vf "crop=639:399:0:0" -c:a mp3 stunts-282ARG-3.mp4 -->
</video>

---

## Restunts

A compiled copy of [Restunts](https://wiki.stunts.hu/wiki/Restunts), a project aiming the rewrite Stunts in C language. The project has been partially completed At the moment some parts have been rewritten in C, while some are still provided by the assembler files extracted from the game executable. My copy has some fixes with respect to the [current Restunts master](https://bitbucket.org/dreadnaut/restunts/src/master/), so this executable should be even nearer to the original game:

* the audio options work, e.g. use `/ssb` for Sound Blaster audio. By copying the appropriate driver files one can also select MT32 (`/smt`) or other supported cards
* the replays are processed correctly, without desyncing

Download:

* <b>[Restunts](/assets/stunts/RESTUNTS-ab5cfab.EXE)</b>

---

## Stunts with binoculars

These mods allow a slightly longer line of sight by sacrificing the side view. The result is much less impressive than what SuperSight can offer, so it is only left here for research purposes.

Here a comparison of the tiles being drawn for a car heading north ($):

```

              O
              O
             OOO
             OOO
 OOOOO       OOO
 OOOOO       OOO
 OOOOO       OOO
 OOOOO       OOO
  O$O        O$O

Original    Modded
  game

```

Downloads:

* [Baseline](/assets/stunts/game.exe) : baseline game.exe. One can diff it with the modded versions to see which bytes were changed
* [v1](/assets/stunts/gamebino.v1.exe) : reorganizes the field of view as shown above
* [v2](/assets/stunts/gamebino.v2.exe) : additionally uses the high-detail model for all objects



