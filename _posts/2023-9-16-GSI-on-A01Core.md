---
layout: post
title: GSI on A01Core
---
I got a Samsung Galaxy A01Core (sm-a013g/ds) as a gift, so I started playing with it a bit.

This phone comes with Android 10 GO STOCK ROM, slow, limited, and full of bloatware.
Because of this, I tested flashing GSI.
It was complicated, because the device doesn't have fastboot and I couldn't get TWRP and the like to work, for some reason. later, a friend shared with me a custom recovery from 4PDA, which supports fastboot.
you can find it [here](https://github.com/AshiVered/Android-custom-ROMs/releases/tag/a01core).
After this step, I saw that the GSI ROM I tested - LineageOS 18.1 by AndyYan, that worked fine on other devices, is very buggy on this device:
1. the SIM card doesn't work.
2. SDcard not work
3. MTP doesn't work.
And this is only what I've tested so far...
Because this, I searched for another ROM.[Someone on XDA wrote](https://forum.xda-developers.com/t/custom-rom-for-samsung-a01-core.4391813/post-88746005) that this device only supports up to Android 10 according to information he extracted from the system, so maybe this causes malfunctions.
So I looked for a GSI 10 ROM, but... I didn't find one that was VNDKLITE - a feature that was important to me for the system to be r/w. So I tried other Android 11 ROMs (after all, the whole point of GSI is to upgrade the device 😉).finally, i found [LiR (LineageOS with pacthes)](https://sourceforge.net/projects/treblerom/files/LiR/2022.03.25/).
I flashed it with fastboot, and it work! (You can see the known bugs, on [my Github](https://github.com/AshiVered/Android-custom-ROMs/tree/main/Projects/Samsung-A01Core-treble)).
Now, I create [tar file for Odin](https://github.com/AshiVered/Android-custom-ROMs/releases/tag/a01core_super) (created with [this tool](https://forum.xda-developers.com/t/installing-gsi-by-repacking-super-img-on-sm-a127f-and-sm-a325f-linux.4365511/)), But I haven't tested it yet. It is available for download here.


