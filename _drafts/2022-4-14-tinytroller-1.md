---
layout: post
title: Tiny-Troller part 1: the path to tinyland

image: why-the-pendulum/pendulum_trajectory.png

---

Let's make tiny motor controller. In this post I'm going to talk about why I am undertaking this project and the design of the first iteration of the PCB. This isn't a tutorial, but it will describe the process I went through in a relatively detailed manner.

### Why would you do this?
This is part of a larger project but we'll talk about that later. The general idea now is that I need to control a small motor, this motor:

![_config.yml]({{ site.baseurl }}/images/why-the-pendulum/tiny_maxon.png)

The dimensions of the motor are (DIMENSIONS) and I got them on ebay. They aren't available on the Maxon website and appear to have originated from the inside of some large piece of medical equipment. They arrived arranged in this curious chunk:

![_config.yml]({{ site.baseurl }}/images/why-the-pendulum/maxon_chunk.png)

I want to turn these motors into little modules: motor, controller, encoder, all in one package for convenient use. So all you have to do is plug in power and comms and start sending commands. The motors come with this little plastic cap on the back which enclosed a small area.

![_config.yml]({{ site.baseurl }}/images/why-the-pendulum/maxon_endcap.png)

The idea which will send us on the surbsequent, circuitous multi-post adventure, was to design a motor controller which could fit inside this endcap...

### Size constraints

Size constraints are the name of the game in this design, so let's start with the board shape, the size constraint to end all size constraints. The board outline in this case is defined by the shape of the volume enclosed in the endcap: a circle with a diameter of (______ mm). Additionally, there are two locating features, two pins which will constrain the rotation of the board (this is essential for getting good encoder readings). The final board shape with some dimensions is shown here:

![_config.yml]({{ site.baseurl }}/images/why-the-pendulum/board_outline_dims.png)

Thickness is also a consideration for this board design, but chips are generally not very tall and I don't have very much control over this dimension.

### What do we need to make this happen?

The system needs a few things:
- Motor driver to control voltage and current to the motor
- Encoder to read the position of the motor
- Communication to send state of the motor (position, velocity, torque), and receive commands from a central computer.
- Microcontroller to manage communication and control the other chips on the board
- Voltage regulation for microcontroller logic level power

So now lets turn these requirements into actual parts we can order. To do this, we will turn to digikey, mouser, arrow, and the like to figure out what our options are. But first, we need to figure out what we're looking for. I like to start with the things that will interface with the board, these constrain the design in a helpful way. Another useful constraint was the package type. I needed really small chips, so I constrained my search for only chips without legs, this left all of the QFN-like packages. These packages are nice because you can still probe the pads with a multimeter after soldering them to check for shorts, and sometimes you can even see the shorted pins under a microscope. For these reasons, I chose to avoid BGAs which are great because of their extremely small size, but hard to check for shorts because all of the pads are on the underside of the package.

On the motor side, the board will output voltage and current to the motor it is controlling. The motor is a brushed DC motor and I would like to drive it with a maximum of 7 volts and 1 Amp. This helps narrow down the search for the motor control chip (which are often specified by voltage and current rating), as well as the voltage regulator for logic power. Whatever input voltage the the motor controller needs will be the input voltage that the regulator gets, so it must be able to handle that voltage. Already we've narrowed it down slightly. For a motor controller, I chose (CHIP NAME HERE) because it was available (chip shortage), and came in a small package.

Now, the other thing the controller interfaces with is a communication bus with potentially many other identical controllers and a main computer which will tell them all what to do. I would like to have this communication bus require very few wires, be easy to add nodes to, and be pretty robust to noise. CAN and LIN are good communication protocols that are options, CAN uses two communication wires and LIN uses only one. As it turns out, some STM32 microcontrollers have CAN or LIN capability, but the only one that was available in the extremely small package I needed was a microcontroller with LIN (the chip shortage narrowed my options significantly). The microcontroller I went with was (CHIP NAME HERE). Another convenient aspect of LIN is that only the main computer needs an accurate clock and all of the child nodes calibrate off that signal before sending a message. This saves us a chip's worth of boardspace and complexity.

So at this point, we were doing LIN. From there, I went looking for chips which could do more than one of the remaining required tasks (to save board space) and as it turns out, you can buy voltage regulators that also act as LIN transceivers and come in a QFN package, the specific chip in question is the (CHIP NAME HERE). Great, that's two chips for the board area of one.

The last chip was the encoder, and for that I wanted to use a coaxial magnetic encoder because of their convenience, all you have to do is put a magnet on the motor shaft and place the encoder coaxially and you're ready to read. These also came in a small package, but were out of stock everywhere. My plan is to build up the rest of the board and firmware using open loop control and then add the encoder when it comes back in stock.

Now we've decided all of the chips, we need to double check that the microcontroller has enough pins. This isn't usually an issue on bigger boards, but when you're trying to use the smallest microcontroller possible, IOs start to be in short supply. In my case, the microcontroller with the 28 pin QFN package had enough pins.

The final step in this phase of the design is to wire everything up with passives according to the datasheets. This includes decoupling capacitors for smoothing out the power signal to each chip (multiple decoupling caps for the microcontroller), as well as current sense resistors and others for the motor controller.

Heres an image (PUT IMAGE HERE) of what the final schematic looks like.

### How do you cram it all in?
Now the actual layout, where we decide where everything actually lives on the board. This step was the longest by far. In fact, in reality, there was a lot of back-and-forth between the layout and the schematic. I'd try to place everything and realize that there definitely wasn't enough room for a crystal oscillator, and then that helped me decide on using LIN. Similarly, the size constraint informed my search for the LIN transceiver/voltage regulator combo chip. All in all, I probably laid this board out 10-15 times.

10-15 times sounds like a lot, and it took a few months of on-and-off work, but importantly, there was no guarantee that everything could fit in the board area I had defined. Sure, you could place the chips inside the outline and say "seems like they fit", but as far as I know, theres no good way to estimate how much area will be taken up by the actual traces which connect everything and the vias which allow traces to go between layers of the board.

At first, I was trying to make this a two-layer board, just a front and a back. It became clear pretty quickly that this wasn't going to give me enough area. I thought about having a stack of two boards, but then the connectors took up so much area on the boards that it didn't really make things better from an area perspective (and made things worse from a complexity perspective).

Making a four layer board was another option. But the regular priced PCB fabricators allow only vias which go all the way through the board. This meant that when I wanted to bring a trace from the front to the back of the board, or from the front to layer three etc, I needed to have a via go all the way through. This meant that it would take up space on both the front and the back of the board. I knew I would need quite a few vias, and that this would eat up my board space quickly. Blind vias are the opposite, they go only between the layers they are required to connect, but this raised the cost of the boards from $100 for 15 boards (pretty reasonable) to $400 for 15 boards which was definitely out of my price range.

Another option which a pursued for a while was to make two boards and solder them directly together. I would put pads on the back of one board and a matching, mirrored set of pads on the back of the other board, then just surface mount them to each other. This would kind of give me the blind vias I wanted so badly, but at the cost of complexity in design and significant complexity in assembly. In the end, I decided to try to cram everything onto a regular four layer board because I guessed that the extra complexity associated with the other methods would make me more likely to get horribly stuck.

I would like to revisit this idea of surface mounting boards to each other though. I thought it would be fun to make a multi board sandwich where each board surface mounts onto its neighbors. Then, if the middle board has cutouts in it, you could have chips sandwiched inside this board stack. It would be a pain in the butt to debug and the soldering might not work, but it sure would be cool (and very efficient from a volumetric perspective).

Still without a guarantee that everything would fit on the four layer board, I pressed on laying everything out. After still more weeks of layout and many failed attempts, it was looking like things were going to fit. I had managed to keep things within the "cheap" tolerances band defined by the PCB house. This meant that my traces couldn't be too close together (otherwise the board became much more expensive). I also used 0402 size passives everywhere because they were large enough that I was sure I could solder them myself, but small enough that many of them could fit on the board. This board would not have been possible using 0603s.

One of the biggest remaining unknowns was how to program the microcontroller. Normally, I'd want to use a USB connector (USB micro or USB-C) for convenience, or even something small JST connectors or something (or a pin header if I'm making something <em>immense</em>), but this board didn't have room for any of those. In fact, as far as I could tell, it didn't have room for any connectors at all...

The thought was to use small test pads which could be placed anywhere on the board, rather than a connector, which afforded no such freedom. I was able to fit the three required test pads for serial wire debug (SWD), but was not able to fit in any of the other features, so my debugging information would be sparse... In the future, it would be nice to be able to fit some of those extra features in for ease of use.

Here's what the board looked like after all the layout was done (BOARD LAYOUT IMAGE)

After this, I spent about a week on-and-off looking for bugs, trying to catch anything before purchased the boards. I didn't find anything major, so pressed the big green button and waited.
