---
layout: default
title: Nucleo Solenoid Shield
description: By Michael Sidler
excerpt_separator: <!--more-->
---

In order to help standardize solenoid-based instruments in WPI's Music, Perception, and Robotics Lab I designed a solenoid shield for STM32 Nucleo development boards.
<!--more-->

![Assembled Solenoid Shield](/assets/NSS.jpg)

There were several hardware requirements to account for while designing this board. The first was the footprint of the STM32 Nucleo F767ZI. The MPR lab is trying to use this development board for all new instruments as it is much more poweful than an Arduino and costs less. All of the required components and connectors must fit on a shield on top of this board and also be compatable with its 3.3 volt logic level.

The next requirement was to have the ability to control at least 25 solenoids. This gives a  range of 2 octaves for any instrument that is created with it.

Finally, the shield should be able to accomadate a wide range of solenoids with different voltage and current requirements. 

<hr>

To meet these requirements, the NTF3055L108T1G MOSFET was chosen to  drive the solenoids. It has a maximum drain source voltage of 60V, maximum continuous current of 3A, and a gate threshold voltage of 2V. Furthermore it is a small SOT-223 package and 25 of these MOSFETs will be able to fit into the required footprint.

An STPS2L60ZFY schottky diode was chosen to accompany the MOSFETs to eliminate flyback voltage spikes when the solenoids are turned off.

The following schematic was used to connect all of the components. `S01-C` is the signal line that controls solenoid 1. `S01-H` and `S01-L` are the high and low sides of the solenoid. There is also a pull-down resistor on the solenoid control line and a status LED.

![Schematic](/assets/NSS_schematic.png)

This schematic was duplicated for each of the 25 required solenoid connections. Headers were also added for the power input and output.

<hr>

Next in the process came the PCB design. This was my first time working in KiCad, so there was a large learning curve with selecting the correct parts and footprints required for the schematic.

I began by placing the edges of the PCB so that the board would be the correct size and then located the headers to the Nucleo within that space. 

Next the power, solenoid connector, and LEDs were placed in accessable positions along the side of the PCB. 

Finally the remaining components were layed out in a grid and traces were routed to all of the components.

![PCB Layout](/assets/NSS_pcb.png)

<hr>

Once the PCBs and components were ordered I began by soldering everything required to control a single solenoid. I applied power and used a function generator to test the single solenoid.

![Single Solenoid Test](/assets/NSS_test.gif)

After the test two full boards were hand soldered.

![Completed Boards](/assets/NSS_finished.jpg)

<hr>

Now that the hardware for this board has been completed example code must be written and the hardware must be tested to ensure that it functions perfectly.

Once everything is confirmed to be working, two additional versions of this board can be made, each utilizing different sets of signal pins from the nucleo. This will allow the three versions of the shield to be stacked onto one nucleo, giving control of up to 75 solenoids! I am looking forward to testing and using this board in future projects.

The hardware source files can be found at: <br>
[https://github.com/MPRlab/nss-hardware](https://github.com/MPRlab/nss-hardware)

The library for the Nucleo board can be found at: <br>
[https://github.com/MPRlab/nss-library](https://github.com/MPRlab/nss-library)
