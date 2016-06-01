---
layout: post
title:  "Creating the Yo Button, Part 1"
date:   2016-04-20
categories: hardware esp8266 msp430
permalink: /Creating-the-yo-button-1
---
A while back I was contacted by my friend [Aria Sabeti][Aria] asking for electronics help on his [Yo Doorbell][doorbell] project. He needed help with the relay. I commented that an esp8266 would be a better fit for the project, and that a Raspberry Pi was overkill. My comment was put to the test soon after, when the founder of Yo saw the project and contacted us to create a hardware Yo Button.

The goal was to make an IoT button similar to the Amazon Dash button: long battery life (at least weeks if not months), usable by a grandmother, and easily setup. Along the way we encountered and overcame several challenges in our design, ranging from technical to regulatory.

1. [FCC certification](#FCC)
2. [USB charging](#USB)
3. [Battery life](#BATTERY)


### <a name="FCC"></a>FCC certification
### <a name="USB"></a>USB charging
### <a name="BATTERY"></a>Battery life
The battery life requirement immediately became an obstacle to using the esp8266. In our initial tests, we could not get the current consumption to be any lower than 40mA while the chip was awake and idling. We could put the chip to sleep and have it wake up 

[Aria]: http://ariasabeti.me/
[doorbell]: http://ariasabeti.me/yodoorbell
[msp430-usb]: http://forum.43oh.com/topic/2962-bit-bang-usb-on-msp430g2452/
