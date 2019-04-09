# touchphat-stream

This script (or collection of scripts) turns a Raspberry PI into a live stream player. It can also play locally stored video files in a loop. It will read a list of streams/videos from a provided file, and start playing a random one. It will then switch between streams depending on input from a Pimoroni Touch pHAT. 

## Disclaimer

This is _not_ what I'd call "good" software. It was written by someone who had never really used Python before, would rather never use it again and generally wants Python to get off his lawn. Quite frankly a lot of this works by brute force. 

## Hardware Requirements

### Raspberry PI. 

I recommend a PI 3 or better. 

#### But I want to use a PI Zero!

Ok... here's what you need:

* Knowledge of how to set stuff up on a PI Zero. The thing has no standard USB ports. Or a standard HDMI port, for that matter. 
* GPIO headers, which the PI Zero doesn't usually come with. So either solder them on, or buy one with headers pre-soldered. You may as well solder them because you'll have to solder later anyway.
* Network. This means either a PI Zero W, or getting some kind of USB dongle. 
* Patience, because this is going to be sssloooowwww. And it ain't exactly zippy on a PI 3 B+.

I know all this because I originally planned on using a PI Zero for this. It works, but...

### [Pimoroni Touch pHAT](https://shop.pimoroni.com/products/touch-phat). 

You _will_ have to solder the GPIO header onto this guy. You'll also need to install the required software; Pimoroni has a very good [Getting Started](https://learn.pimoroni.com/tutorial/sandyj/getting-started-with-touch-phat) page which explains how to do both. 

### MicroSD Card

Duh, right? I'm only mentioning this because your diskspace obviously limits how many local videos you can store. 

## Software Requirements

### The aforementioned Pimoroni software library

Instructions on installation are available on their website, and are pretty straightforward. 

### [streamlink](https://github.com/streamlink/streamlink)

Installation instructions are, once again, available directly from the project page, and are also straightforward.

### omxplayer

Can be installed via ```apt-get install omxplayer```

### This stuff. 

git clone this thing. 

## Raspberry PI Configuration

* Install the latest version of [Raspbarian Lite](https://www.raspberrypi.org/downloads/raspbian/). 
* Let it do the initial setup.
* Run ``sudo raspi-config`` and *change the default passord*, set up networking, enable ``ssh`` (if you want to) etc. Might as well do an ``Update``, too. 
* While in``raspi-config``, select ``Advanced Options`` -> ``Memory Split`` and set the GPU RAM to 256 MB. 

## Configure ``syslog``

If you want to see any kind of output on the console, add the following line to ```/etc/rsyslog.conf```:

```*.notice                        /dev/console```

Since the script runs at reboot, this is the only consistent way to provide output. 

