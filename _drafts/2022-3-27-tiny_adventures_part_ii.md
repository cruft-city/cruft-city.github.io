---
layout: post
title: Tiny Troller&#58; adventures in tinyland part II, to shave a yak 

image: tiny_troller_1/shorted_swclk_on_micro.png

---

All those test pads on that tiny motor controller (LINK TO FIRST POST) weren't originally intended for solder. This kinematically coupled, pogo pin powered, magnetically clasping bad boy was always part of the plan. I just had to absolutely drain all of the patience out of my body re-soldering programming wires a million times before I actually got around to making it, but here we are. Now instead of making progress on the board, let's yak shave away a spring break making a fun little programming jig.

Here's a look at what's inside:

(EXPLODED VIEW GIF)

The kinematic coupling (three little spheres and corresponding grooves) allow for easy and precise aligment of the jig whenever it is assembled. This is convenient because the pads on the motor controller are 1mm in diameter and we really want the pogo pins to land on their corresponding pads every time. We don't need ultra-precision, but I'd certainly rather not worry about it. Plus, it's not everyday that we can get that nice, perfectly constrained, tingly feeling. There will be no elastic averaging in my programming jig! Except ironically, because of the pogo pins (and the deformable layer lines from the printer at a smaller scale), I guess you could say that there is actually a lot of elastic averaging going on, but we're not going to think about that. For now, kinematic coupling = happy precision feeling, but we'll come back to that.

The jig is printed in PLA on the Prusa MK3s. The printer does an excellent job with the relatively tight tolerances and thin features of the jig. After a couple of iterations, the pogo pins could be pushed in without violence or heat, but also removed without destroying either pin or print. Nice. The magnetic clasping between the top and bottom pieces of the jig was a lot stronger than I expected and it means you have to be careful when putting the top of the jig on, but my main concern was that they generated enough force to compress the pogo pins on both the top and the bottom. They do this and more. Another benfit of the magnets is that, despite the relatively symmetrical design of the jig, the magents can be installed with north up for two of them and south up for the third, this makes things simple because the top will only go on one way. Soldering the thin wire onto the end of the pogo pins was a bit of a challenge, but nothing too hard for some trial and error. I ended up bending a little loop into the end of the wire which helped make the job easier. Another challenge was keeping the solder on the flat end of the pogo pin. Any extraneous solder changed the OD of the pin and it would no longer fit in the tightly toleranced hole where it was going to live.

Before programming the microcontroller, I had to make sure this jig is actually going to work... You may notice that the pogo pins are extremely close together. So close in fact, that they make contact. This is a bad thing, if the pins short, we'll be back where we started, but now with two things to debug rather than one. But, so far the resistance between pogo pines has been in the mega ohm range, which is tolerable. Screens from the oscilloscope below show the difference between the raw signal (measured at the pins of the programmer with nothing connected), and at the jig:

![_config.yml]({{ site.baseurl }}/images/tiny_troller_1/st_link_no_tiny_programmer_attached_signal_control.png)

(Above) control: programmer with no jig attached (clean signal)


![_config.yml]({{ site.baseurl }}/images/tiny_troller_1/tiny_programmer_v1_validation_no_board_looking_good.png)

(above) programmer with jig attached (messy signal)


It is clear that the signal is less clean with the programmer attached, but half the reason to use digital systems is noise immunity right? Some of the signal from the SWCLK pin can be seen bleeding over into the SWDIO signal, but the amplitude is low, so it shouldn't matter for the performance of the device. If for some reason, any of this becomes problematic, it can be diagnosed by repeating the tests shown above. The design of the programming jig can also be changed so that the extra-tightly-spaced pogo pins are angled between 5 and 10 degrees. Because they are only close to touching at the base (the widest point) if they are angled slightly away from each other, this problem can be avoided. I didn't do this for the first version of the jig because it means that the contact position of the pogo pin changes in the plane when the board moves vertically. Since the board is more or less suspended between two sets of pogo pins, its exact z position could change based on varations in friction between the board and the locating pins (bumpy layers from the printer, etc). Opting for simplicity in keeping the pins straight and taking on the slightly higher risk of shorting has proved to be fine, at least so far.

A not on the alignment repeatability of the kinematic coupling: (REPEAT THE KINEMATIC COUPLING LAB FROM 2.750 HERE, ALSO RELATE IT TO THE SIZE OF THE LAYER LINES FROM THE PRINTER).

Now back to what we initially set out to do, program the microcontroller. With shiny new jig in hand, we shamble onwards towards our goal. Keep it locked for part 3 coming to a blog near you where we actually do something with this little guy.