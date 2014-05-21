Welcome to vcompress!
=====================

----------

Summary
---------

This is a small wrapper script (and installation documentation) that I came up with when I had to compress a larger number of videos for a client. My MacBook Air was giving me render times of like 24 hours for one hour of video. Using the open source software ffmpeg - and figuring out how to make it work and give a same quality result - **I was able to reduce the render time from 24 hours to 15 minutes** and hence also avoid buying another solution - e.g. a MacPro - for like 8k. The lazy person I am, I decided to script the whole process. Further below is a little more history on the setup that I use, but let's get at it.


This script - **vcompress** - I share here for you to hack away on.

----------

Installation
---------

Copy **vcompress** to your linux box. Optionally, you can look towards the end of the script where I have put, in a comments section, the steps I took to install ffmpeg and related.

----------

Usage
---------

```
./vcompress
Usage: ./vcompress [options] input.mov output.mov

-h| --help                 :  Print this help.
-s| --start 3              :  Start encoding at 3 seconds.
-d| --duration 4           :  Encode only 4 seconds.
-t| --threads 4            :  Use 4 threads. Default: 8.
-r| --resolution 1920x1080 :  Change resolution. Default: keep.
-a| --aspect 16:9          :  Change aspect ratio. Default: keep.
-b| --bitrate 9000k        :  Bitrate (quality). Default: 9000k.
-p| --passes 2             :  Use 1 or two passes. Default: 2.
-f| --flash                :  Also generate .flv file. Default: don't generate.
-l| --loglevel             :  Verbosity. Default: warning. Other values: quiet|panic|fatal|error|warning|info|verbose|debug
```

Which means, in it's easiest form, you can just call it like so:

```
./vcompress input.mov output.mov
```

And if you want to have generated a flash file with it, you say

```
./vcompress -f input.mov output.mov
```

This will do a two-pass compression - and optionally an added flv conversion - of your input.mov:

```
# ./vcompress -f input.mov output.mov
============================================================================
Starting conversion at 14:18:58.
============================================================================
Length: 00:00:01.00 - Frames: 24 

Frame: 23 of 24 Time: 00:00:03 ETA: 00:00:00 Percent: 95
----------------------------------------------------------------------------
0 minutes and 3 seconds elapsed for the first pass.
----------------------------------------------------------------------------
Length: 00:00:01.00 - Frames: 24 

Frame: 23 of 24 Time: 00:00:04 ETA: 00:00:00 Percent: 95
----------------------------------------------------------------------------
0 minutes and 4 seconds elapsed for the second pass.
----------------------------------------------------------------------------
Length: 00:00:01.00 - Frames: 24 

Frame: 0 of 24 Time: 00:00:00 ETA: 0 Percent: 95
----------------------------------------------------------------------------
0 minutes and 2 seconds elapsed for the flv conversion.
----------------------------------------------------------------------------
============================================================================
0 minutes and 9 seconds elapsed in total.
----------------------------------------------------------------------------
Ended conversion at 14:19:07.
============================================================================
```


----------

History
---------


Hello. I recently had to edit another bunch of videos which I had recorded, for a client, using my [Canon 7D][1], using my [Canon SX 50HS][2] as a backup and second angle, my [Miller DS-20 tripod][3] and a [Roland R-26][4] audio recorder. The setting was two days of group workshop, in-house. In particular, I couldn't use lapell microphones as they'd been too intrusive. That's where the Roland R-26 makes a very good job over the built-in microphones of the cameras.

This leaves you then with several hundreds of gigabytes of data, five audio sources in total, and hence a lot of cutting to be done on my MacBook Air. While the importing, audio synchronization and cutting can be done relatively easily on the MacBook Air, when it comes to actually compressing the videos, as you can imagine, this is not the right device to do it. A one hour video may well take 24 hours to complete, and during that time, the MacBook will burn it's way through your desktop.

So my classic solution to that was this: I also do have a relatively sizeable server, about 8 cores, 64 GB of RAM and massive amounts of harddisks on multiple RAIDs, that I had built for me by [Silicon Mechanics][5]. This normally serves to run the about 20 virtual machines I normally operate with using my development, testing, innovation, etc. work for my day job. The good thing about this is that is has massive power, and it runs on Linux.

The bad thing is a somewhat cognitive dissonance I had developed over the years, favoring strongly Apple hardware when it comes to video editing, and Linux for everything else. 

So when a while ago I had to do more video editing, I went as far as installing Mac OS inside a [Virtual Box][6], only to then discover, that FinalCut Pro refused to start up due to lack of proper video cards. Well, yes. That's a VM after all, but then I just want it to do the export and compression job. In a moment of despair, I tried calling Apple's compressor on that machine, and while it gave the same error message as FinalCut, it did not exit, but then stayed running.

Which worked, quite well, in the sense that I now exported, with no compression, from my MacBook Air the videos once I had cut them, and let Compressor, out of a VM, do the compression job. It still took about 24 hours, but I couldn't care less because during that time I could do something else.

That model works until your next client gives you just large enough a number of videos to cut through, and just very short time to do it - 24 hours, then, are by no means acceptable.

It is an interesting path dependency of thoughts, if you think about it: I use Linux as primary operating system since about 1993. But assumtions you make, such as "Apple's better at video cutting" will easily create a working environment (though, "easily" is not exactly the right word if you think about the complexity of making OSX run inside a VM). But that added complexity then creates sunk costs, i.e. you move even further into where you should never have gone.

In other words, while my client is "breathing down my neck" I decide to question my assumptions as to the underlying problem - if I can come up with a problem to the video compression time fast enough, I'll catch up with the time lost on that investigation easily.

So my assumption being that I have to use Apple Compressor for compressing videos that come out of Apple's FinalCut, I removed that assumption. Which led me to having to look for another video compression software.

Which, of course, is [ffmpeg][7]. The issue with that software is probably again that you first of all have to compile it manually to have all the conversion options available that you need in order to get to an output similar to that of Compressor; the next issue then is the more or less thousands of options you have to parameterize the program.

So I went through the options I had, did a lot of Googling, made the software run (how I did is in the comments section of the vcompress script) and then wrote a very simple to use shell script that allows you to invoke the compression on a video you export from FinalCut and have it compress - and optionally as well converted into a .flv (for platform independent serving).

