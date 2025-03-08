# Magic Clock for Home Assistant

If you ever read or saw Harry Potter, you may remember the Weasley Family's magic clock that showed the whereabouts of all family members.
And of course you wanted something like that, too.

**Good News**: I built one and can share my work with you.

![Magic Clock Final](magic_clock.jpg)

**Bad news**: You probably need to adjust each part of my work to your requirements. And it's several parts that need to work together.

The clock was meant to give my wive who had to go overseas for several months a way to connect back to me and home by getting live updates, but also make her presence felt by switching the living room light colors.

## Overview

This project uses a ESP32 to control a servo motor connected to a clock dial to display a persons current status. That status is based on several inputs like GPS data from phone, calendar data, a sleep state sensor or currently switched on Hue devices.

A local Home Assistant instance collects those data and decides where to point the clock dial, indicating locations like office or school, but also current activities like sleeping.

In addition, intended as a really simple way of two-way conversation, two switches control two LEDs that can be toggled on or off. The LED status is relayed back to the Home Assistant server and can be monitored and controlled from there, too.

## Requirements and Sub-Projects

It turned out that you need to combine several trades to build a magic clock. On the other hand, none of them went into the really advanced stuff and all the tools needed are free and Open Source.

- _[Smart Home Platform](homeassistant/)_: Home Assistant is used to collect all necessary data and control the clocks servo motor.
- _[CAD and 3D-Printing](CAD/)_: The case for the clock was designed using FreeCad
- _Firmware and Network_: The EspHome-Project is far the easiest way to generate a firmware for ESP32 or similar that connects to Home Assistant
- _[Hardware](schematics/)_: Some sketches of the schematics
- _Graphics & Design_: Templates and measurements to help you designing your front plate for the magic clock. I used Scribus.

Detailed descriptions can be found in those folders.

## 3D-Printing

STL-Files for 3D-Printing the case will be available on printables.com

I will be happy to see your results or comments there.

## Disclaimer

This is my personal hobby project and it is completely tailored to what I had already running, knew how to do or was ready to learn. Feel free to use my work as inspiration for your own and I would love to learn what you ended up making.
