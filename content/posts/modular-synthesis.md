# Introduction to modular synthesis

### [Francesco Cameli](https://github.com/vitreo12)

---

During this hackspace I gave a brief introduction to modular synthesis. 
I first started by giving an overview on the differences between digital and analog audio, outlining the strengths and weaknesess of each. Then, I showed my eurorack setup and explained the principles that drove me when choosing the modules I own. Finally, the focus was expanded to the creative possibilities that modular systems offer when used in conjuction with digital audio environments like [SuperCollider](https://supercollider.github.io/), [Max](https://cycling74.com/) or [pd](https://puredata.info/).

---

## Digital vs analog
 
There is one fundamental difference between analog and digital audio: **quantization**. While on one hand analog signals are the result of **continous** changes in **voltage**, digital signals, on the other hand, happen at **discrete** moments in time - called **samples**. The amount of samples per second determines the **sampling rate** of a digital system: a sampling rate of 44100 Hz explictly specifies that 44100 samples will be generated every second by our system. The higher the sampling rate, the higher will be the resoultion of the resulting audio signal, as more samples per second will be produced, reducing the risk of unwanted effects such as [digital aliasing](https://en.wikipedia.org/wiki/Nyquist_frequency).

{{< image "/img/modular-synthesis/digital-vs-analog.png" "analog vs digital signals" >}}

All considered, what are the advantages and drawbacks of each? First of all, there is the notion of cost to take into consideration. Analog technology, while having better sonic fidelity, is considerably more expensive than its digital counterpart, as it needs the production of a single piece of hardware that it's capable of only the one task it's been created for. Digital systems, on the other hand, can often run on our everyday computers, without requiring any additional purchases. This also links to the problem of reproducibility. Audio software can easily repeat the same set of commands as long as the CPU can perform, allowing inexpensive - money wise - execution of, for example, 100 sine tones at the same time: definitely easier and cheaper than buying 100 hardware sine tone generators and patch them together!

---

## The eurorack format

Eurorack is a specific format for modular synthesizers originally developed by [Doepfer](http://www.doepfer.de/home.htm). The size of modular systems, generally,is expressed in terms of U (around 128mm) for height and HP (around 5mm) in length. In the case of eurorack modules, the height is fixed at 3U, while the length can vary from usually 2HP up to 40HP. As any other modular system, eurorack modules are housed in cases with different amount of rows. Eurorack modules draw power from a two-row ribbon cable connector containing either 10 or 16 pins, and they call for ±12V power, with some modules also requiring +5V. While eurorack is the most commercially common format, it's not the only one. Other popular formats are [Buchla](https://buchla.com/modular-systems/)'s and [Serge](https://randomsource.net/serge)'s.

{{< image "/img/modular-synthesis/eurorack-case.jpeg" "an euorack travel case" >}}

As with other modular systems, eurorack synthesizers center around the concept of patching cables from the various modules, in order to create complex voltage interactions that will then produce the desired sounds. As previously hinted, being modular systems often completely analog, the voltage exchange between the modules does not follow a rigorous **discrete** order, but it all happens simultaneously. This allows for complex cross-modulations that are not possible in the digital world, as they cannot be expressed as a sequence of ordered commands, especially when introducing feedback networks in the patching (e.g. modulating a filter's cutoff control pitch with its own output).

---

## Hybrid audio: digital meets analog

It is possible to create hybrid systems that can leverage the ease of digital audio manipulation and the complexity and richness of analog eurorack sinthesizers. The use of DC coupled audio interfaces, or, alternatively, eurorack modules like the [Expert Sleepers ES-8](https://www.expert-sleepers.co.uk/es8.html), allow the possibility of sharing audio streams between the eurorack system and a computer, opening up for even more complex interactions between the two worlds.

This is a picture from my setup which is built around the concept of being a noise generating machine. All the modulations are then routed from the computer to the system via the ES-8 module. On the digital side, my software of choice is SuperCollider programming language.

{{< image "/img/modular-synthesis/eurorack-view.jpg" "my eurorack setup" >}}

To make the system work, I have setup a simple shell scripts which executes several commands in series. These commands allow me to boot a `jack` server which aggregates my two soundcards: an old Scarlett 2i2 and an Expert Sleepers ES-8. The command on the left boots the main jack server on the 2i2 with a high scheduling priority (also, note, I'm using a real-time patched linux kernel to be able to set custom priorities on certain processes) with a sampling rate of 48000 samples and a vector size of 128 samples. On the top right, I'm using the `jack_load` command to link the ES8 to the running server, effectively creating an aggregate device using both the soundcards. Finally, after booting the SuperCollider server, I'm executing the command on the bottom right, `aj-snapshot`, which is a nice utility to restore saved jack connections. Of course, I developed my own shell script that does all these things, so that I can only call `audio_setup` (that's the name I gave the script) and have everything booted up and ready to play with. 

{{< image "/img/modular-synthesis/jack-boot.png" "boot the jack server" >}}

As an example, here a cubic interpolated noise function ( `LFNoise2.ar` ) is sent to the first output of the ES-8, and then used as FM input for the [Instruo CS-L](https://www.instruomodular.com/product/csl/) oscillator. In my setup, the outputs from SuperCollider to the eurorack system correspond to values `2..18` (16 in total), hence why here `Out.ar(2, ...)` is used. The first line of code ( `Out.ar([0, 1], ...)` ) simply plays the output from the eurorack system to the speakers. 

{{< image "/img/modular-synthesis/sc-screen.png" "supercollider screenshot" >}}

{{< image "/img/modular-synthesis/es8-fm-view.jpg" "the es-8 connected to the cs-l" >}}