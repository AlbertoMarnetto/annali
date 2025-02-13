---
title: "Stunts Modification Projects"
layout: default
---

# Stunts

This page contains my mods of Stunts (aka 4D Sports Driving). Their source code is available in [my Restunts repository](https://github.com/AlbertoMarnetto/restunts).

Note that all mods require a copy of the original game. This was declared freeware by the original developers, and can be obtained in the [download section](https://wiki.stunts.hu/wiki/Download) of the Stunts Wiki.

---

## SuperSight

A mod increasing the field of view to a radius of up to 10 tiles around the car and drawing all objects in high resolution. For very crowded scenes, it automatically reduces the detail to avoid running out of memory. Additionally it adds a couple of functions:
* F5 turns on/off the display of the 3D engine status
* F6 turns on/off the visibility of [illusion tiles](https://wiki.stunts.hu/wiki/Illusion_track)

**Download:**
* <b>[SuperSight v1.5](/assets/stunts/RESTUNTS-10a1e10.EXE)</b> (2025-02-11)

**Making of:**
* [**Part I**](/2025/02/20/broderbund-stunts-1.html) : general introduction and the making of “Stunts with binoculars”
* **Part II:** the first releases of SuperSight (WIP)
* **Part III:** wrong ways and failed experiments (WIP)
* **Part IV:** the final polish (WIP)
* [**Discussion**](https://forum.stunts.hu/index.php?topic=4400.msg) on the Stunts forum – the mod production “as it happened”

**Demonstration video:**
<video controls width="640">
  <source src="/assets/stunts/stunts--282ARG.mp4" />
  <!-- ffmpeg -i *.mkv(om[1]) -vf "crop=639:399:0:0" -c:a mp3 stunts--282ARG-3.mp4 -->
</video>

---

## Stunts with binoculars

This mod, precursor of SuperSight, allows a slightly longer line of sight by sacrificing the side view, as described in my [writeup](/_posts/2025/02/20/broderbund-stunts-1.html). The result is much less impressive than what SuperSight can offer, so it is only left here for didactic purposes.

**Download:**

* [Stunts with binoculars](/assets/stunts/gamebino.exe)

---

## Restunts

A compiled copy of [Restunts](https://wiki.stunts.hu/wiki/Restunts), a project aiming the rewrite Stunts in C language. My copy has some fixes with respect to the [current Restunts master](https://bitbucket.org/dreadnaut/restunts/src/master/):

* the audio options work, e.g. use `/ssb` for Sound Blaster audio. By copying the appropriate driver files one can also select MT32 (`/smt`) or other supported cards
* the replays are processed correctly, without desyncing

**Download:**

* <b>[Restunts](/assets/stunts/RESTUNTS-ab5cfab.EXE)</b>


---

## Vanilla Stunts

This the original game, as provided by the Restunts project. It differs from the commercial editions in that it removes the multi-stage loading and the copy protection. 

**Download:**

* <b>[Stunts](/assets/stunts/game.exe)</b>

