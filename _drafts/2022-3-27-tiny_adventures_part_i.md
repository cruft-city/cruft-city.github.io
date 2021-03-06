---
layout: post
title: Tiny Troller&#58; adventures in tinyland part I, on the road to tinyland

image: tiny_troller_1/shorted_swclk_on_micro.png

---

Talk about designing the board and soldering it up:
what is the purpose of the board
walk through each major component
Fitting everything in (design)
Pushing traces feature in kicad is nice
four layer
talk about soldering everything on

The width of a usb mini connector is about half the width of the whole board, and the connected length would be even longer. Even pin headers or small JST connectors are out of the question due to footprint. I didn't even have room to put all the pads for programming next to each other, let alone have additional room for a connector. So what we're left with is a bunch of test pads on the perimeter of the board. 

When I assembled the first few boards, I attempted to solder on standard 1.15mm (17 AWG) wire onto the 1mm diameter test pads. Although I was able to attach the wire, the wire ripped a test pad off of the board. I was able to briefly solder the thinner 0.3mm wire into one of the vias which remained in the pad, but it didn't last. 

(PICTURE OF WIRE SOLDERED ONTO VIA)

So with the test pad down I had to assemble another board and this time used the 0.3mm wire for all connections. These have worked well and no further pads have been lost. I soldered an ugly harnass of 0.3mm wire attached to standard size prototype wire and connected that to an ST-LINK liberated from the top of a nucleo. As a side note, I bought some prototyping wire kits which have pre-crimped M-M, M-F, F-F wires and the little blocky plastic connector parts. These are very nice because the wires can be mixed and matched to make whatever combination of connector is required, and then removed from the connector blocks and re-used when the connector has served its purpose. I bought mine from Pololu -although I am sure they are sold elsewhere- and it has been a major quality of life improvement.

(PICTURE OF PROTOTYPING WIRE KITS)

Anyway, another major lesson I learned with these tiny buggers is to get the solder stencil. It's just for the QFN microcontroller packages. I can solder the big QFNs and the passives on all day, but oh boy that high pad density QFN microcontroller has been a huge pain to get working correctly. I keep adding too much paste and shorting some of the pins together. At first I thought there was something wrong with my layout, I had tried touching up the microcontroller soldering job a few times and was reasonably sure it was correct, but still I couldnt connect. When I looked at the wires on the scope, I could see the programmer sending its messages, and I could even see the microcontroller trying to respond, but the signal was tiny. It had to be a short.

![_config.yml]({{ site.baseurl }}/images/tiny_troller_1/shorted_swclk_on_micro.png)

Re-positioning the microcontroller is a piece of cake, just blow some hot air on it and poke it with the tweezers. The frustrating part of the whole ordeal was that each time I blew hot air on the board, the programming wires I had painstakingly soldered on came off. So each time I wanted to check the solder on the micro (and believe me, there were MANY times, and will no doubt be many more), I had to completely remove and re-do the soldering of all four tiny programming wires. All this to say, all good projects need some yack shaving, see part II (LINK TO PART 2).