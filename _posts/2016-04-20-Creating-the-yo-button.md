---
layout: post
title:  "Creating the Yo Button, Part 1"
date:   2016-04-20
categories: hardware esp8266 msp430
permalink: /Creating-the-yo-button-1
---
A while back I was contacted by my friend [Aria Sabeti][Aria] asking for electronics help on his [Yo Doorbell][doorbell]. I commented that an esp8266 would be a better fit for the project, and that a Raspberry Pi was overkill. My comment was put to the test soon after, when the founder of Yo saw the project and contacted us to create a hardware Yo Button.

The goal was to make an IoT button similar to the Amazon Dash button: long battery life (at least weeks if not months), simple interface, and easy setup. Along the way we encountered and overcame several intermixed challenges in our design, ranging from technical to regulatory. We ended up not finishing the project, but I'm documenting it anyways.

1. [FCC certification](#FCC)
2. [USB charging](#USB)
3. [Battery life](#BATTERY)


### <a name="FCC"></a>FCC certification

filler text

### <a name="USB"></a>USB charging

more filler text

### <a name="BATTERY"></a>Battery life

The battery life requirement became an obstacle to using the esp8266. Initially, we could not get the current consumption to be lower than 40mA while the chip was awake and idling. The obvious solution was to put the chip to sleep and have it wake up when the user pressed the button. That would have been the way to go if not for two things: the trigger that the ESP needed to wake up from sleep, and the need to detect short and long presses.

The button component pulled a pin low when pressed, but the ESP required a rising edge on its RST pin to wake up from deep sleep. I could have worked around this with a couple of resistors and a capacitor on a separate GPIO pin, but we had the MSP co-processor for USB negotiation and battery charging so we decided to decouple the button detection from the ESP.

In the end, both the ESP8266 and MSP430 defaulted to deep sleep. A button press event would wake up the MSP430 which in turn would wake up the ESP8266 and signal a long or short press.

On deep sleep, the ESP8266 draws ~10 uA, and the MSP430 draws an astonishingly low 0.1uA in LPM4. When active, its an entirely different story, with the ESP8266 drawing as much as 300mA in short bursts as it communicates to the wireless access point.

Choosing the power supply regulator was not a trivial task. My go-to prototyping 3.3V regulator, the LD1117V33 has a quiescent current of 5mA! In other words, it would be consuming 500x as much power as the processors during deep sleep! But any low I<sub>Q</sub> LDO would need to be able to handle the 300mA bursts of the ESP8266. Any LDO with truly low I<sub>Q</sub> was unable to handle the relatively high current requirement (eg: the [TPS783 from TI][tps783]) After some searching I found the [Micrel 5504][micrel] which fit the bill nicely with an I<sub>Q</sub> of 38 uA and capable of delivering 300mA, all in a small SOT-23 package.

With these components our deep sleep current was estimated to be 50 uA, we unfortunately never got to actually measure it. With our target battery size of 680 mAH, the button would have lasted over 9000 hours! More than a year! 

[Aria]: https://ariasabeti.me/
[doorbell]: https://ariasabeti.me/yodoorbell
[msp430-usb]: https://forum.43oh.com/topic/2962-bit-bang-usb-on-msp430g2452/
[micrel]: https://www.microchip.com/wwwproducts/en/MIC5504
[tps783]: https://www.ti.com/lit/ds/symlink/tps783.pdf
