---
title:
layout: default
---

# SuperSight: a graphical enhancement mod for Brøderbund's Stunts

The project uses tricky and fragile tricks to piece together `asm` and `c`, but miracolously the system works. The documentation is very good and the build system works well: after spending time trying to rewrite the makefiles to work with Linux, I find that it's much easier to just follow the intended workflow and run `make` from a Wine console.

<center>
<a href="/assets/stunts/stunts--wineconsole.png">
<img src="/assets/stunts/stunts--wineconsole.png" alt="Running the build system from Wine" width="600"/>
</a>
<figcaption>Wine is a wonderful tool, but its console is a horror. It has no tab completion, the command history is non persisted and worst of all, pressing CTRL+C closes the console instead of stopping the program running in it.</figcaption>
</center>
