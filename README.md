# touchphat-stream

This script (or collection of scripts) turns a Raspberry PI into a live stream player. It can also play locally stored video files in a loop. It will read a list of streams/videos from a provided file, and start playing a random one. It will then switch between streams depending on input from a Pimoroni Touch pHAT. 

## Disclaimer

This is _not_ what I'd call "good" software. It was written by someone who had never really used Python before, would rather never use it again and generally wants Python to get off his lawn. Quite frankly a lot of this works by brute force. 

## TODO / Wishlist

* This file is a barely legible monstrosity. 
* More comments in the code. Like, _any_ comments. 
* The configuration files should prooobably be unified. And be JSON or YAML or something because that's what all the cool kids are doing. Or XML because that'd annoy people. 
* The code can be streamlined a lot. Especially towards the end. The end of me writing it, not the end of the file. You know, when I was getting lazy. Lazier. 

## Hardware Requirements

### Raspberry PI. 

I recommend a PI 3 or better. See below (way below) if you want to use a PI Zero. 

### [Pimoroni Touch pHAT](https://shop.pimoroni.com/products/touch-phat). 

You _will_ have to solder the GPIO header onto this guy. You'll also need to install the required software; Pimoroni has a very good [Getting Started](https://learn.pimoroni.com/tutorial/sandyj/getting-started-with-touch-phat) page which explains how to do both. 

### MicroSD Card

Duh, right? I'm only mentioning this because your diskspace obviously limits how many local videos you can store. 

## Software Requirements

### The latest [Raspbarian Lite](https://www.raspberrypi.org/downloads/raspbian/).

* Install normally, and let it do the initial setup.
* Run ``sudo raspi-config`` and *change the default passord*, set up networking, enable ``ssh`` (if you want to), localisation (if you need it), etc. Might as well do an ``Update``, too. 
* While in``raspi-config``, select ``Advanced Options`` -> ``Memory Split`` and set the GPU RAM to 256 MB. 

### The aforementioned Pimoroni software library

Instructions on installation are available on [their website](https://learn.pimoroni.com/tutorial/sandyj/getting-started-with-touch-phat), and are pretty straightforward despite being the much-maligned curl|bash method.

### [streamlink](https://github.com/streamlink/streamlink)

This program can stream livestreams from a variety of services. 

Installation instructions are, once again, available directly from the project page, and are also straightforward. 

### omxplayer

This is the actual video player. It is used by streamlink, and to directly play locally stored videos.

It can be installed via ```apt-get install omxplayer```

## Configure ``syslog``

If you want to see any kind of output on the console, add the following line to ```/etc/rsyslog.conf```:

```*.notice                        /dev/console```

Since the script runs at reboot, this is the only consistent way to provide output. 

## Install This Stuff.

Download the zip or ``git clone`` the thing (note that you'll have to ``apt-get install git`` for that one). 

Intallation is assumed to be in ``/home/pi/touchphat-stream``. Because that's friggin hardcoded. 


## But I want to use a PI Zero!

Yeah, that was my original plan, too. I had a PI Zero that was happily looping a local 1080p video so I figured, what the hell. And a PI Zero will happily stream a 1080p YouTube stream. 

*However*. 

* Setting up a PI Zero is slightly more complex than a "regular" PI. 
** The thing has no standard USB ports, so if you want to use a keyboard you need a USB2Go dongle, or to setup Ethernet-over-USB by editing stuff on the MicroSD card before booting it up, or setting up WiFi on the MicroSD card, or use a USB Ethernet dongle... etc.
* It has no standard HDMI port either, for that matter. So you need a Mini HDMI to HDMI adapter. Or is it micro HDMI?... one of those. 
* You need GPIO headers, which the PI Zero doesn't usually come with. So either solder them on, or buy one with headers pre-soldered. You may as well solder them because you have to solder the headers on the touchphat, anyway. 
* You need to be online, which means you need the PI Zero W, or (again) a USB Ethernet dongle.  
* If you want audio and you're not plugged into something that supports audio over HDMI, the thing has no 3.5mm speaker jack. 
* It. is. going. to. be. slooooooow. Slow to start, slow to switch streams, just sllloooowww. And it ain't exactly zippy on a PI 3 B+. Now once a video or stream _start_, you're good. But time between those... 

Again, it _will_ work. But it's more work and it is slower. 

However, it _does_ look kinda cooler to have a _tiny_ thing doing all this. 
