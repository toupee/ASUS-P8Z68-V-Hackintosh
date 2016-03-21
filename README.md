# ASUS-P8Z68-V-Hackintosh
My notes on making this motherboard and parts Hackintoshable. So far, so good.

Thanks to many many people for getting me up and running, especially u/corpnewt (Reddit), u/macmeister (Reddit), DavidGoldman (Github), tkrotoff (Github), Mirone (Github), and so many others.

I found that there were very few guides that were especially helpful in undertaking this little project. Everything is made out to be so simple. Create a USB stick with Unibeast! Throw Multibeast on there! Boot 'er up! S/L/R! Kexts! DSDTs! Umm... figuring all this stuff out was actually a lot harder than I expected. There's still a ton I don't know. But hopefully this guide will help some folks with the same setup as me get up and running in a couple hours, rather than a couple dozen hours like it took me. (Weekend Warrior? More like... Week-long Warrior.)

In any case, this is *sort of* written for the first-time Hackintosh builder -- because that's where I'm coming from. You'll probably find some of my definitions are outright incorrect, but it's how I came to understand their functions. I definitely welcome any edits and clarifications!

This guide is for OS X 10.11 El Capitan.

# My setup

I think it's important that I share my build. I think the most important parts are:
* ASUS P8Z68-V Motherboard (NOT PRO/GEN3 or LX)
* nVidia GTX 970
* Samsung 840Evo SSD (I decided to completely format and use as the install location)
* 16GB MicroSD card in a Rocketek adapter is my boot disk, which I just leave inserted as it has Clover on it

Probably less critical, but:
* 24GB of RAM
* Several other hard drives (NTFS) - they are accessible, but not writable
* A WDMyCloud NAS hard drive, which is readable and writable
* An external USB 3.0 hard drive, formatted for OS X
* 600W power supply
* Razer Chroma keyboard (sometimes doesn't work in BIOS, strangely; I keep a second keyboard on deck)
* LG IPS LED 23EA63 monitor (no issues)
* Seiki SE39UI04 4k monitor (will crash system if on during boot; turning on after boot, no issues! Weird I know.)

# Motherboard Setup
I did update my BIOS Version to 3402 by loading the ROM file from Asus's website onto a blank USB drive, going into Advanced in my BIOS and running the EZFlash2 utility. It then searches the USB stick for the BIOS and updates. I did NOT patch the file, although I wasted a ton of time trying to figure out how to do it. It wasn't necessary. I don't even know if updating my BIOS to this version was, to be honest.

# USB Stick Preparation
Things you'll need:
* A copy of El Capitan, downloaded from the App Store on a legit Mac. There are possibly other ways to get it, but this is the easiest IF you have access to a Mac... I did. It plops a copy in the Applications folder.
* Unibeast - this is an app that takes that El Capitan install file and mutates it into a bootable disk. Make sure you have a version of Unibeast that supports El Cap, and just click Yes Yes Yes to Clover and UEFI install.
* Hidden EFI Partition -- What wasn't clear to me, and screwed me for hours upon hours, is that Unibeast (should) create an EFI partition on that boot disk. You can mount this partition by opening terminal, typing diskutil mount /dev/disk0s1 (Make sure you double-check your disk number and partition number by typing diskutil list in terminal; disk0s1 is typically the EFI partition, but there are some systems out there that have their drives managed differently]. Check the kexts folder. You MUST have FakeSMC.kext and NullCPUPowerManagement.kext in there for the installer to boot correctly!!
* Add cpus=1 to the boot arguments. You can do this with CloverConfigurator or just do it in Clover and go to Options when it boots. This supposedly makes your system more stable by limiting the # of CPU cores it will use. It also makes your system considerably slower. Be sure you remove this boot argument when you actually have El Capitan installed; it took me a few days to realize this, wondering why it seemed relatively slow compared to Windows, and then I found my system was FLYING afterwards.

I've also found that some USB sticks were simply borked. If you've tried all that and still can't get to the installer... try another stick.

Also, I would use a USB 2.0 port instead of a USB 3.0 port. I know it's slower, but I think it alleviated some potential issues.

# Things I needed to know about Clover
Clover is basically the boot management system that starts the installer for OS X and, after it's installed, allows you to boot into it. There are a couple of configurable elements to consider. One is the kexts folder which loads what are essentially drivers upon boot. Another is boot arguments, such as cpus=1 or -v, which is verbose mode (shows what the system is doing when booting).
config.plist is the file that tells Clover what to do by default. You can edit this file in a text editor or by using a GUI tool like CloverConfigurator, and you can also override config.plist when Clover is actually booted by going into the Options menu.

Kexts are loaded based on the folder Clover expects it to boot, so make sure it's 10.11 -- or just copy the kexts into the "other" folder (/EFI/CLOVER/kexts/other/) whose contents get loaded regardless of the OSX version.

* These are the boot arguments I used when installing OS X: -v -cpus=1 nv_disable=1
* And these are the boot arguments I use now that it is installed (along with the nVidia graphics driver): -v dart=0 nvda_drv=1

* Another note courtesy corpnewt: Make sure your Clover config.plist contains:

<key>RtVariables</key>
<dict>
    <key>CsrActiveConfig</key>
    <string>0x3</string>
    <key>BooterConfig</key>
    <string>0x28</string>
</dict>

There may be other keys in the RtVariables section, but you need to at least have the CsrActiveConfig and BooterConfig keys and their respective values. They set SIP (System Integrity Protection) to a partially disabled state that allows for the loading of third-party kexts.

(Thanks buddy!)

# Installation
Before I figured all this stuff out, the installer would constantly freeze for me as soon as it went from verbose mode to the gray/tan screen. But when I was able to continue through all the installation dialogs, I was good to go! However, it took a Very long time. I actually gave it overnight to install.
* For some reason, the Log was spammed with Language-thing, and some have claimed there is a fix for this, but I found that it only caused me more errors. I just let it take its sweet time. It looks like it stalls about 10 minutes in. Just let it go.
* Ensure that cpus=1 is still enabled when your computer reboots for part 2 of installation! It might fail otherwise.

# Post-Install: Graphics
nVidia Web Drivers did the trick for me. It pretty much instantly boosted my resolution to the expected, and seems to allow games to run decently well. I can tell you that in my VERY quick and amateur testing of running a recent (pretty) game, Firewatch, it does NOT run as well in OS X as it does in Windows. It still is quite playable, but noticeably not as smooth. [On the other hand, Spelunky runs perfectly; see below]

# Things to know about kexts after installing
KextBeast + Kext Utility is your friend. Remember those kexts that we put on the EFI partition? They get loaded when you boot the system, but there's a more permanent place to put kexts. That would be (jargon) S/L or S/L/E. That means either the System/Library folder or the System/Library/Extensions folder. The following two kexts both go in S/L/E.

KextBeast basically just looks for kext files you temporarily place on your desktop and installs them to one of those two locations. You'll then need to refresh your "kext cache" by running the Kext Utility. There are other ways to do this, but this was a reliable method for me.

# Post-Install Kext: Audio
This motherboard has ALC892 Audio. This was actually the final piece I needed to get working, and I tried a bunch of different crap, but all you need to know is to grab the Mirone AppleHDA.kext and HDAEnabler1.kext and install them with KextBeast (both to System/Library/Extensions) and run KextUtility afterwards to update the cache. 

# Post-Install Kext: USB 3.0
GenericUSBXHCI.kext - Do the same as above to install. This should get your USB 3.0 ports working. I still have a problem where anything on them is ejected when the system goes to sleep, unfortunately.

# Bonus: Spelunky
One of the few Windows-only games I wanted to try and get running on OS X is Spelunky. (If you haven't tried it, it's great fun.) This involved delving even deeper into some jargon and strange program interfaces. Here's what I did to get it working.
I mostly referenced this Reddit thread (https://www.reddit.com/r/spelunky/comments/1jzevu/new_spelunky_works_on_os_x_using_wineskin/) but with u/shamansir's comments. Since that thread is locked, here's my interpretation of what I actually had to do, with updated version numbers for 2016:
(Warning: this text is incomplete as of 3/22/16)
* Download Wineskin Winery 1.7. Wineskin Winery is sort of a command center for making "wrappers" (read: executables) which apply certain conditions to folders that contain Windows programs. Upon opening Wineskin Winery, update the Wrapper Version to the latest (mine is Wineskin-2.6.2.)
* Add WS9Wine1.9.2 -- Engine means "Select WS9Wine1.2.2 engine in the list and push Create Blank Wrapper button and name it somehow to recognise it easily in future (it will be the .app file).
* Make a new wrapper using the 1.9.2 engine  means "[from the Wrapper you've created] Go to 'Wineskin Advanced', [find 'steam' and 'd3dx9' in the list] and push 'Install Software' button'".
* In winetricks, d3dx9
* Some say they can install Steam using winetricks but that did not work for me. What also didn't quite work was downloading the latest version of Steam for Mac and running it to install. It got stuck in an endless cycle of updating. At this point, I actually copied the entire Steam directory (minus steamapps) from my Windows PC and copied it all into the Steam folder. At this point, it actually successfully opens.
* Make sure you have the xbox360ce drivers in the same folder as Wineskin. You can always get back to this by right-clicking your wrapper and 'Open Package Contents.' I had to muck about with the buttons in the Xbox360ce.ini file for my Xbox One controller. I've figured out the buttons: A=1 B=2 X=3 Y=4 LB=5 RB=6 Select=7 Start=8 LC=9 RC=10
* You MIGHT have to install the Xbox One driver for your Mac in general for the controller to work at all (link) I have encountered a bug where if I unplug my controller when OS X is running, it can crash my system instantly. This also happens on a legit Macbook Pro in my experience.
* Install Spelunky through Steam and it should work!
* You can then set the Wrapper to launch Spelunky directly using a path like:
* Add a nifty Spelunky icon to replace the Wineskin one!
* Caveats: It takes a bit longer to open because when you launch the program Steam has to start from scratch and checks for updates. Spelunky also always says it's installing DirectX drivers for the first time, but then it just jumps right into the program and I don't have to deal with Steam bugging out as it randomly seems to running this way. The whole process takes 22 seconds for me. The game runs amazingly well. Upon exiting, it might require an Alt+Tab to get out of a black screen and a force quit to truly end it, but I've had no issues result from this.
* ENJOY! Try this out with other games ... I have no idea what else works with this method.

# Persisting Issues
USB 3.0 is a little sketchy. I have an external hard drive attached to one that seems to eject every time the computer goes into sleep. (The good news is, sleeping works!)

# Other Fun Stuff
Now that you have Mac OS X up and running, you should try out some of these cool programs! These are the reasons I wanted OS X at all...
* Karabiner - This tool will allow you to configure your keyboard. I set Alt key to Command, set Home and End to do what I expect, and disabled Eject.
* f.lux! - Gradually removes blue light from your displays as you get closer to bedtime. Not great for color matching but it does help me fall asleep a bit faster! Try it!
* OpenEmu! - a VERY nice piece of software that displays ROMs in a gorgeous format with box art and alleviates the need to download a half a dozen different emulators with different setups.
* Affinity Designer and Affinity Photo! - This was honestly the main reason I wanted to run OS X. These pieces of software are intended to replace Adobe Illustrator and Adobe Photoshop, and they do a DAMN fine job - I actually prefer them now. So many tools are just easier to use, the interface is really polished, and exporting files is a BREEZE. Also, they're way cheaper - you can get each one for $40 on sale and there is no subscription BS. (Funnily enough, they announced this software is coming to Windows just a couple of weeks after I succeeded in this Hackintosh project... well... I guess that's nice.)
* Final Cut Pro X! - I'm not totally converted from Premiere Pro yet, because I'm an old hand at that, but there are some really innovative things going on here that make it nice to launch for quick projects (and possibly more extensive projects).
* Logic Pro X! - It's just a touch more intuitive than REAPER (free and awesome) - a very polished piece of audio editing software. It's very nice for multi-track editing and recording. It doesn't have the audio editing capabilities of Audition, but it's a far nicer tool for laying out a track.
* Finder - Let's face it, Windows Explorer feels like its stuck in the 1990s, with no tabs, an overloaded shortcuts bar, an overloaded "ribbon" (and I do like the ribbon in Office), no Column view. El Cap also has a fantastic Rename feature. (Windows does do thumbnails a bit better, I'll give it that.)
