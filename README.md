# touchphat-stream

This script turns a Raspberry PI into a live stream player. It can also play locally stored video files in a loop. It will read a list of streams/videos from a provided file, and start playing a random one. It will then switch between streams depending on input from a Pimoroni Touch pHAT. 

## Disclaimer

This is _not_ what I'd call "good" software. It was written by someone who had never really used Python before, would rather never use it again and generally wants Python to get off his lawn. Quite frankly a lot of this works by brute force. 

## Hardware Requirements

### Raspberry PI. 

I recomment a PI 3 or better. 

#### But I want to use a PI Zero!

Ok... here's what you need:

* Knowing how to set stuff up on a PI Zero. The thing has no standard USB ports. Or a standard HDMI port, for that matter. 
* GPIO headers, which the PI Zero doesn't usually come with. So either solder them on, or buy one with headers pre-soldered. You may as well solder them because you'll have to solder later anyway.
* Network. This means either a PI Zero W, or getting some kind of USB dongle. 
* Patience, because this is going to be sssloooowwww. And it ain't exactly zippy on a PI 3 B+.

I know all this because I originally planned on using a PI Zero for this. It works, but...

### [Pimoroni Touch pHAT](https://shop.pimoroni.com/products/touch-phat). 

You _will_ have to solder the GPIO header onto this guy. You'll also need to install the required software; Pimoroni has a very good [Getting Started](https://learn.pimoroni.com/tutorial/sandyj/getting-started-with-touch-phat) page which explains how to do both. 

## Software Requirements

### The aforementioned Pimoroni software library

### [streamlink](https://github.com/streamlink/streamlink)

### omxplayer

Can be installed via ```apt-get install omxplayer```

### Configure syslog

Add the following line to ```/etc/rsyslog.conf```:

```*.notice                        /dev/console```

Since the script runs at reboot, this is the only consistent way to provide output. 
