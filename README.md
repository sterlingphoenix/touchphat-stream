# touchphat-stream

This script turns a Raspberry PI into a live streaming device. It will read a list of streams from a provided file, and start playing a random one. It will then switch between streams depending on input from a Pimoroni Touch pHAT. 

# Requirements

1. Raspberry PI. 

This _will_ work on a PI Zero, but it will be pretty slow when switching streams. You'll also need to provide network connectivity (via USB or the WiFi on a Pi Zero W), and either buy one with the GPIO headers pre-soldered, or solder them in yourself. A Raspberry PI 3 is a lot simpler. 

2. [Pimoroni Touch pHAT](https://shop.pimoroni.com/products/touch-phat). 

You _will_ have to solder the GPIO header onto this guy. You'll also need to install the required software; Pimoroni has a very good [Getting Started](https://learn.pimoroni.com/tutorial/sandyj/getting-started-with-touch-phat) page which explains how to do both. 

