# Business card

## The idea

The first time I ever heard of a PCB business card was just by chance.
Back then, an engineer named Frank Zhao made one that could be plugged into a computers' USB port.
It presented itself as a keyboard and typed his contact info into a fresh notepad window.
Pretty much immediately, I fell in love with the concept of special business cards.

Since then I've seen a couple of different and very interesting designs.

What inspired me most to try and create my own however was Ilia Baranov's design.
While not offering any sort of functional electronic circuitry itself, Baranov's design was uploaded to Altium CircuitMaker, which at the time was my PCB design software of choice.
The second I came across it, it caused a click in my brain.
Until then, something like this seemed like something an accomplished engineer with a degree would do after years of working in a big company.
I realized that literally anyone could create something like that.
It made me realize that what I always wanted to do was ready for me to grasp.

## Developing a concept

### Initial draft

I immediately started drafting.
Collecting data and over-thinking things a lot.
The rough shape was the first thing I worked on.
This eventually turned out to be the only property to make it into the final version.

My initial plans were quite ambitious. I wanted to create a plug-and-play USB drive.
Something any PC could recognize just like a regular thumb drive and store my resume on.
This idea eventually had to be scrapped however for the following reasons:
* No simple, integrated, single-IC solution was readily available.
I would have had to start from scratch with a microcontroller and NAND-flash memory.
* The cost would have been too high.
* Furthermore, this task would have been to difficult.
I thought I'd lack the expertise, especially in terms of software.
* Most companies nowadays employ a "no foreign storage media" policy in order to not introduce potential malware to their system.
I'm aware that keeping IT systems secure is a big deal in corporate environments, meaning my card would potentially not have been used as intended.

### Final version

After realizing my previous idea wouldn't have been optimal, I went back to the drafting board again.
Over the course of a couple months I gathered inspiration from various projects around the internet.
What stood out pretty clear was the fact that if I were to put a circuit on my business card, it would only have to draw power and otherwise stand on its own.
After pondering about it for a bit, I realized that a school project I did in the past could be what I was looking for all this time:
**a simple game of Roulette**.

This approach has the following advantages:
* It was enough to show my expertise with microcontrollers
* It was easy enough to do in a reasonable amount of time
* It wasn't too costly
* The parts were easy to come by
* It's timeless and something that can always be enjoyed. Like rolling dice.
* It has lots of LEDs. Therefore, this also allowed me to test out how well a thin PCB would work as a diffusor.
* I wanted to try out a circuit with capacitive touch or something similar.
* I wanted to try a stacked PCB design. This idea I got from Marco Reps teardown of the PIRL USB charger.

## Circuit board design

### Schematic

Creating the schematic was a piece of cake.
Thanks to my experience with various previous projects based on either the well-established Arduino UNO or its centerpiece, the ATmega328, I was able to piece together the basic circuit on a piece of paper in less than 30 minutes.

All of the components were picked with availability in mind.
That is to say, I deliberately chose to use off-the-shelf components.
To my surprise, German distributor Reichelt happened to have all that I needed.
That includes the ATmega48P, which is virtually the same IC with the only difference being 4 KB of program memory instead of 32 KB.
I was also able to acquire all other components I needed from them.

My choice of EDA software was Altium CircuitMaker due to its easy of use and me already being familiar with it from previous projects.
Entering the schematic into Altium took around an hour.
In the end I came up with this:

<!--TODO:-->

### PCB layout

#### Determining stacked PCB count

As previously stated, I planned to stack PCBs similar to the <!--TODO:--> PIRL charger.
<!--TODO:-->

#### Board shape

The first thing I did was determining the size and shape of the business card.
Fortunately, Wikipedia offers information on pretty much anything, including common formats for business cards.
Neat!
I went with a size of 85mm by 55mm, which is common here in Germany.

With the boundaries set, I went for the corner that plugs into the USB port.
This turned out to be surprisingly hard to determine, because there barely seemed to be any information on the exact measurements of USB ports.
In the end, I just grabbed a random USB thumb drive I happened to have nearby and measured it with a pair of calipers.
This gave me a height of 2.4 mm and a width of <!--TODO:--> mm.
I ended up eyeballing the length at <!--TODO:--> mm.
Slightly risky, but I was confident it was going to work out well.

#### Bottom PCB layout

I figured that I should probably take care of the hardest part first, which was the bottom PCB hosting all SMD components.

Since aesthetics were of very high importance, I decided to roughly place the components first.
I already had a good idea of what I wanted to do.
The centerpiece obviously being the microcontroller sitting in a cutout.
All LEDs were placed around the MCU in a perfect circle.
That meant that all of them had to be rotated in increments 27.69 degrees (360 / 13).
Initially, this proved to be a rather difficult undertaking as the footprint of the LEDs had their origin in one of their corners.
To solve this problem, I ended up writing a script that would add two vectors together to calculate the X and Y coordinate of each LED.
The script however didn't help since my math skills were a bit rusty and I kept receiving values that were off.
It was at this point that I realized there's a simpler solution.
I could just move the origin in the middle of the footprint by modifying the model.

During the first half of the design process, I didn't put any effort into routing the components together.
I planned to just use the autorouter eventually and maybe drag some of the traces around to make everything look nicer.
But the longer I looked at the PCB, the more I felt the urge to make the circuit single-sided.
This would have meant a very clean look, as no through-holes would have been necessary.
I became so obsessed with the idea, that I gave it a shot.
To my own surprise I was actually able to pull it off after a considerable amount of extra effort.
While this resulted in some sub-optimal routing that would make every engineer scream -- such as unnecesarily long traces and an ungrounded back plate --, I was confident it would end up working fine due to the relative simplicity of the circuit.
Lastly I added a few empty solder pads as "test points" to allow for programming.

#### Top PCB layout

While the top PCB doesn't have any electronic components, I had to pay special attention to semantics.
Everything should be self-explanatory upon first glance.

Only two things have to be on the top PCB: the USB port and the touch button area.

The touch button area took a bit of tinkering and testing out, but in the end I came up with something that was easy to understand, functional and didn't look too bad.
It is fully printed on with one of its sides being grounded.
The other side is constantly pulled up to 5V by a 1 M <!--TODO:-->Ohm resistor and attached to one of the ADC-pins of the ATmega.
Touching it creates a bridge between ground and the pin, pulling the voltage down thanks to the relatively low resistance of human skin.

The USB port is what caused a headache to me.
As the USB 2.0 pinout is among the most iconic things in the world, I'd argue that nearly everyone is able to recognize a USB device simply by looking at the four pins.
My design however only uses two of the pins for power.
I didn't want to put the two data pins on the PCB itself in order to explicitely convey that the PCB does not act as a potentially malicious device.
After some consideration I decided to just outline their position with the silk mask.

The big hole in the middle of the PCB is self-explanatory.
For the roulette game, I put a circle on top of every LED in the <!--TODO:-->Keep-out layer.
This meant that neither solder mask nor copper would be on the PCB in that area.

#### Middle PCB layout

The sandwiched middle-layer was by far the easiest part.
It only featured a bare PCB (no copper, no silk, no solder mask) and a few holes.

The most obvious hole is the big one in the middle where the ATmega sits. However, I also put in various holes to hide components and to give everything a cleaner look.

#### Aesthetics

While I initially planned to have a regular “green” PCB, the idea of making something more premium and fancy eventually crossed my mind.
While selecting a PCB manufacturer, I happened to stumble upon PCBway.
One of their offers was a "matte black" option.
Despite never having seen with my own eyes up until this point, I remembered how amazing it looked on the "Nixie Clock" from Dalibor Farny.
While gold-plated on a matte black PCB meant a sharp increase in manufacturing costs (around 200€!) I immediately fell in love with the idea.
Additionally, a gold plating would help against corrosion from fingerprints.

The back side was a different deal.
I wanted it to be a very sharp contrast towards the front side.
Since I knew it should be colored white, I took some unusual inspiration: a can of Monster Energy Zero Ultra.
The black and silver on white with embossed decals always made the beverage stand out for me.

## Ordering

### PCBs

Ordering at PCBway was easier said than done.
Aside from the risk of accidentally clicking the wrong options, I feared that the middle part could be too weird of an order.
I guess that a PCB without any copper and only holes was bound to raise some questions.
And what do you know, it did!
I swiftly got a message from PCBway who thought I had forgotten to upload all files.
After a bit of forth and back with Amaris, I convinced them that *yes, I do indeed want a PCB with only holes in the middle*.
Shoutout to them for their great customer service.

For the top PCB, I chose a matte black finish with gilded contacts. For the middle PCB, I chose the "no mask" option. The lower PCB had a white finish with no drills. Additionally, I ordered a stencil for the lower PCB.

### Components

Usually I wouldn't have to explain ordering components.
However it is worth noting that I had to pay special attention to component height due to the exceptional constraints.
Luckily, I could just order off-the-shelf components from Reichelt, like I originally intended.

### Price

All things considered, I paid
* €188.17 ($208.00) for printing 30 PCBs,
* around €50 for customs,
* and €75.75 for roughly 15 sets of components.

This results in a final **unit cost of €11.25** if we don't consider in one-time purchases like solder paste.
Not too bad!

## Assembly

### Soldering

This was straightforward. At the time I received all ordered parts, I was lucky enough to still be at my old job at Gepro.
I could simply apply the solder mask with the provided stencil, throw my PCBs into one of their ovens and call it a day.
Two weeks later and I would have had to solder everything by hand.

### Programming

Since I was already familiar with the Arduino IDE, programming wasn't too much of a challenge.

```C++
const uint8_t lights[13] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 14, 15};
uint8_t bias = 0;

void stall() {
  //Stall until analog pin exceeds treshold voltage (VCC/2)
  while(analogRead(16) > 819);
}

void setup() {
  //Declare outputs
  for(byte i = 0; i < sizeof(lights); i++){
    pinMode(lights[i], OUTPUT);
  }
  //Inputs do NOT need to be declared
  //Seed the randomness generator with the 3 unused analog input pins
  uint32_t seed = analogRead(17);
  seed += analogRead(18) << 8;
  seed += analogRead(19) << 16;
  randomSeed(seed);
  //Stall code execution until first button press
  stall();
}

void flash(uint8_t light, uint32_t delay_ms) {
  digitalWrite(light, HIGH);
  delay(delay_ms);
  digitalWrite(light, LOW);
}

void loop() {
  uint32_t delay_ms = 10;
  const uint8_t result = 53 + random(sizeof(lights)) + bias;
  for(uint8_t i = bias; i < result; i++){
    uint8_t light = i % sizeof(lights);
    flash(lights[light], delay_ms);
    delay_ms += pow(8., ((i - bias) / 64.));
  }
  bias = result % sizeof(lights);
  digitalWrite(lights[bias], HIGH);
  delay(1000);
  stall();
  digitalWrite(lights[bias], LOW);
}
```

The only thing I underestimated was the massive size of the `pow()` function, which ended up taking a whopping 3.5 KB of the available 4 KB of program memory.
Whoops!
While this would have been something to immediately optimize in a real product, I was simply too impatient to improve this flaw.
Any professional would probably pull their hair out for essentially wasting this much memory and CPU time, but hey: it works!
Guess this is how it's gonna stay now.

### Stacking PCBs

What I imagined to be

## Takeaway
Things to do better next time / Flaws / 

* Hole above TVS-Diode
* Zener-Diode for Touchsense
* HASL (no ENIG) for bottom PCB
* Larger copper clearance around numbers
* Texture for bottom side of bottom PCB (preferably working circuit) similar to "Monster Zero Ultra" can
* Ground bottom side of bottom PCB
* Add vias for ground of bottom PCB to avoid swing
* Pads not on top of each other for connecting so solder blobs don't prevent gluing together
* Bigger holes for cramming wire (fear of flexing PCB)



