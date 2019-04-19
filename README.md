# touchphat-stream

This script (or collection of scripts) turns a Raspberry PI into a live stream player. It can also play locally stored video files in a loop. It will read a list of streams/videos from a provided file, and start playing a random one. It will then switch between streams depending on input from a Pimoroni Touch pHAT. The Touch pHAT has six keys -- "Back" and "Enter" are used as Forward/Backward in the stream list. Keys A through D are used as hotkeys for specific streams. 

Also note that "stream" refers to either a stream or a local video, because I'm lazy. 

On startup, the script will load a provided stream file, randomise the list and start playing at a random spot. 

After it sleeps for 5 seconds. Because I think it messes up the network test otherwise. 

## Disclaimer

This is _not_ what I'd call "good" software. It was written by someone who had never really used Python before, would rather never use it again and generally wants Python to get off his lawn. Quite frankly a lot of this works by brute force. 

## TODO / Wishlist

* `README.md` is a barely legible monstrosity. 
* The configuration files should prooobably be unified. And be JSON or YAML or something because that's what all the cool kids are doing. Or XML because that'd annoy people. 
* The code can be streamlined a lot. Especially towards the end. The end of me writing it, not the end of the file. You know, when I was getting lazy. Lazier. 

## Hardware Requirements

### Raspberry PI.

I recommend a PI 3 or better. See below ([way below](#but-i-want-to-use-a-pi-zero)) if you want to use a PI Zero. 

### [Pimoroni Touch pHAT](https://shop.pimoroni.com/products/touch-phat). 

These don't come pre-assembled, so you will have to solder the GPIO header onto the board. You'll also need to install the required software; Pimoroni has a very good [Getting Started](https://learn.pimoroni.com/tutorial/sandyj/getting-started-with-touch-phat) page which explains how to do both. 

### MicroSD Card

Duh, right? I'm only mentioning this because your diskspace obviously limits how many local videos you can store. 

## Software Requirements

### The Latest [Raspbarian Lite](https://www.raspberrypi.org/downloads/raspbian/).

* Install normally, and let it do the initial setup.
* Run `sudo raspi-config` and *change the default passord*, set up networking, enable `ssh` (if you want to), localisation (if you need it), etc. Might as well do an `Update`, too. 
* While in`raspi-config`, select `Advanced Options` -> `Memory Split` and set the GPU RAM to 256 MB. 

### The Aforementioned Pimoroni Software Library

Instructions on installation are available on [their website](https://learn.pimoroni.com/tutorial/sandyj/getting-started-with-touch-phat), and are pretty straightforward despite being the much-maligned curl|bash method.

### [streamlink](https://github.com/streamlink/streamlink)

This program can stream livestreams from a variety of services. 

Installation instructions are, once again, available directly from the project page, and are also straightforward. 

### omxplayer

This is the actual video player. It is used by streamlink, and to directly play locally stored videos.

It can be installed via `apt-get install omxplayer`

## Configure `syslog`

If you want to see any kind of output on the console, add the following line to `/etc/rsyslog.conf`:
```
*.notice                   /dev/console
```

Since the script runs at reboot, this is the only consistent way to provide output. 

## Install This Stuff.

Installation is assumed to be in `/home/pi/touchphat-stream`. 

You'll need to install `git` on your PI (`apt-get install git`). 

Then, from `/home/pi`, run

`git clone https://github.com/sterlingphoenix/touchphat-stream.git`

### Included Files

* `touchphat-stream` - the main script. 
* `streamer_helper_script` - helper shell script that starts the stream/video. Since live streams tend to, well, crash, it makes sure to keep things going.
* `cleanup_helper_script` - helper shell script that kills _everything_ with some degree of vehemence. Makes sure new stream doesn't merge with new stream. Ever see pandas running around a beach in Hawaii? That was a fun bug. 
* `syslogger` - script that takes whatever you throw at it and barfs it out to syslog. 

### Configuration Files

* `streams.dat` - main file containing streams to cycle through.  
* `key_a.dat` through `key_d.dat` - these contain a stream for each corresponding hotkey. Separate files (for now) so they can either be included or not in the main list. 

Configuration files are basically a comma-separated list with the format

`Stream URL/Path to Local File,Description`

Super simple, right? Also has no provision for comments or, well, empty lines. Or errors... ugh. 

A stream URL is the full URL, including the https:// bit. A local video is the full path to the videos. You can store them wherever you want, but I recommend a `videos/` directory within the main installation. 

## Test It! 

*Note*: testing the Pimoroni Touch pHAT is detailed on their installation page, and you should absolutely make sure that thing works. 

While you can start this script locally, it's probably a better idea to do so via `ssh` since you'll still see some output the console will otherwise obscure. Basically, run:

    /home/pi/touchphat-stream/touchphat-stream
    
You should see something like this on the console:

    Apr 10 12:02:42 localhost /touchphat-stream: Hi there!
    Apr 10 12:02:42 localhost /touchphat-stream: Waiting for network to be available.
    Apr 10 12:02:45 localhost /touchphat-stream: Network is up!
    Apr 10 12:02:45 localhost /touchphat-stream: 16 streams detected.
    Apr 10 12:02:45 localhost /touchphat-stream: Playing stream #4: Kittens!!!
    
Your ssh session should have some output from the helper scripts and video player applications, so any errors will show up there. Streams that no longer exist, or any typos, can break this. 

If you want to abort the script to fix any errors, you can kill it with a `^c`, but make sure to run `/home/pi/touchphat-stream/cleanup_helper_script` to make sure there are no lingering players. 

You'll want to cycle through all your streams and try all the hotkeys before setting the thing to run on boot.

## Setting The Thing To Run On Boot

Run `sudo crontab -e` (you may be prompted to choose an editor, choose the one you're comfortable with and it better be `vim`).

Go to the end of the file and add:

`@reboot /home/pi/touchphat-stream/touchphat-stream`

Save the file, and reboot. *Done!*

---
    
## But I want to use a PI Zero!

Yeah, that was my original plan, too. I had a PI Zero that was happily looping a local 1080p video so I figured, what the hell. And a PI Zero will happily stream a 1080p YouTube stream. 

*However*. 

* Setting up a PI Zero is slightly more complex than a "regular" PI. 
  * The thing has no standard USB ports, so if you want to use a keyboard you need a USB2Go dongle, or to setup Ethernet-over-USB by editing stuff on the MicroSD card before booting it up, or setting up WiFi on the MicroSD card before booting it up, or use a USB Ethernet dongle... etc. There are guides todoing all this, but it's a pain in the butt. If you _are_ going to try, a USB2Go dongle is probably the simplest way. 
* It has no standard HDMI port either, for that matter. So you need a Mini HDMI to HDMI adapter. that's "mini", not "micro". Because why not have different standards with names that are basically synonyms.  
* You need GPIO headers to connect the Touch pHAT, which the PI Zero doesn't usually come with. So either solder them on, or buy one with headers pre-soldered. You may as well solder them because you have to solder the headers on the Touch pHAT, anyway. 
* You need to be online, which means you need the PI Zero W, or (again) a USB Ethernet dongle.  
* If you want audio and you're not plugged into something that supports audio over HDMI, the thing has no 3.5mm speaker jack. 
* It. is. going. to. be. slooooooow. Slow to start, slow to switch streams, just sllloooowww. And it ain't exactly zippy on a PI 3 B+. Now once a video or stream _start_, you're good. But time between those... 

Again, it _will_ work. But it's more work and it is slower. 

However, it _does_ look kinda cooler to have a _tiny_ thing doing all this. 
