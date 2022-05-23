---
layout: post
title: The secret life of the pendulum

image: why-the-pendulum/pendulum_trajectory.png

---

When I first started learning about dynamic systems and controls, I saw pendulums everywhere. As far as I could tell, pendulums had no real practical use significant enough to warrant this intense level of academic scrutiny. I knew I was missing something, so I went under cover and learned about dynamics and controls. Finally, here it is: the tell all, the pendulum exposé...

![_config.yml]({{ site.baseurl }}/images/why-the-pendulum/pendulum_trajectory.png)

### What is a pendulum?

This might be obvious, but for the sake of starting off on the right foot, let's begin at the very beginning. The place where it all began. What is a pendulum? A pendulum is a thing that can rotate around a joint. That's maybe too general, but we usually find pendulums oriented vertically, like an arm hanging from a shoulder, so hopefully that narrows it down. Although as we will see later, sometimes it's helpful to take a broad view about what qualifies as a pendulum. Unfortunately, that may not have been as enlightening as I'd hoped it would be so I've attached a few images of common pendulums that should clear up any lingering ambiguity before we forge ahead into unknown lands. 

PICTURE OF PENDULUM HERE

The pendulum is a simple thing, conceptually at least. Which is part of why it is so important not only to dynamics and controls, but to clocks and also arguably to cavemen. One of the earliest known scientific uses of the pendulum was a seismograph (a device for detecting earthquakes) invented by the royal astronomer Chang Heng in the year 132 AD in Han Dynasty China. This device used an inverted pendulum (a pendulum precariously balanced upside down), which fell and swung whenever the gound shook. When the pendulum fell, it dislodged one of a few balls arrayed around it to indicate the direction the earthquake came from [1]. Chang Heng's pendulum is the first known scientific use of a pendulum, but bolas, slings and similar absolutely ancient weapons function on a similar principle and were incredibly fashionable technologies as recently as 6,000 BC [2]. Pendulums also make great clocks as we will talk about shortly. What makes the pendulum so useful as a clock, as a weapon, or as a device for detecting and amplifying the smallest of tremors is a concept called <em>resonance</em>.

### Resonance and the pendulum as clock and sling

Somewhere after pendlums detected earthquakes in the palaces of ancient China and before they appeared in dynamic systems problem sets for engineering a physics students, the humble pendulum was the beating heart inside of the most accurate clocks ever built. In fact, Gallileo studied the timekeeping ability of the pendulum as early as 1583, and even described to his son Vincenzio how a pendulum based clock could be made, but the clock was never made. Instead, the first pendulum clock wouldn't arrive for another 74 years until 1657 when the illustrious Christian Huygens designed and built one. Between 1657 and sometime in the 1920's or 1930's, pendulum clocks were the most accurate way to tick away the passing seconds [3]. 

As mentioned previously, the pendulum is useful for timekeeping because it <em>resonates</em>. At a high level, resonance is vibrating and every object does it no matter how large or small. The frequency of the vibrations depend on the size of the object and every object has a so called resonant frequency. The resonant frequency is the frequency that you'll find that particular object vibrating at after it is hit or otherwise imbued with some form of energy or other. Bigger objects have lower (or slower) resonant frequencies, and smaller objects have higher (or faster) resonant frequencies. Some things are so big that the resonance looks like waves and we can determine the individual peaks as they move slowly (or stand still in the form of a standing wave). Some things are so small and they resonate so fast that we cannot even perceive their vibrations such as atoms (although we cannot see atoms anyway, so it doesn't make much difference). Stiffness plays a role as well. Stiff things have higher resonant frequencies and floppy things have lower resonant frequencies. Air and fluids can also resonate, and the resonant frequency of these depend on the size of the container that they are in. The fundamental idea of resonance is that, when you add energy to the system, it shows up as vibrations at the resonant frequency. When you are swinging on swingset -- grotesquely transformed into yet another pendulum -- no matter how hard you pump your legs, you will always swing at the same frequency. You cannot pump harder to increase the <em>frequency</em> of your swinging, you can only increase the amplitude. There may be many frequencies present in an excited system, or there may only be the resonant frequency, this depends on other paremeters of the system. In the case of a swing, only the resonant frequency exists. 

Pendulums make good clocks and slings because any energy you add goes straight into exciting the system at the resonant frequency. Any energy added to the pendulum of a clock goes pretty much exclusively into moving the pendulum bob at a fixed frequency. Because the resonant frequency of the pendulum depends entirely on it's geometry (how long it is and how much it weighs), the resonant frequency will stay almost exactly the same (only changing with thermal expansion of the pendulum). Energy can be lost through friction at the joint, or friction with the air, but if more energy is added, it will not change the frequency of the clock. This is extremely handy for keeping time. Pendulums are also useful as slings for the same reason. A slinger moves their wrist in a vaguely oscillatory manner and all of this energy goes pretty much directly into spinning the sling around the slinger's arm. The energy transfer is so effective that with only a few oscillations of the wrist, a sling can be rotating 
 
# natural frequency vs resonant frequency vs first mode
in both theory and practice, the natural frequency, resonant frequency, and first mode all mean slightly different, very specific things and I'm not going to go into detail about that becuase I want to keep this ~ f ~ u ~ n ~, but check out the references at the end of the post for more in depth writing about this topic. Also check out the Wikipedia artcles for natural frequency etc.

[1] - https://www.uh.edu/engines/epi324.htm

[2] - Rosenberg, Danny. “Flying Stones – The Slingstones of the Wadi Rabah Culture of the Southern Levant.” Paléorient, vol. 35, no. 2, [Paleorient and CNRS Editions, CNRS Editions], 2009, pp. 99–112, http://www.jstor.org/stable/41496871.

[3] - https://web.archive.org/web/20110717061023/http://www.ieee-uffc.org/main/history.asp?file=marrison