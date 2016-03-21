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

# Motherboard Setup
BIOS Version
No patching necessary

# USB Stick Preparation
Things you'll need:
* Unibeast
* Hidden EFI Partition + How to mount
* Kexts
* cpus=1

Some USB sticks were simply borked.

Also, I would use a USB 2.0 port instead of a USB 3.0 port. I know it's slower, but I think it alleviated some potential issues.

# Things to know about kexts before installing

# An explanation of boot arguments
boot args
10.11
unecessary flags I tried

# Things I needed to know about Clover
config.plist
CloverConfigurator

# Installation
Took a very long time
Log was spammed with Language-thing, but trying to fix it did not work
Ensure that cpus=1 is still enabled for part 2 of installation!

# Post-Install: Graphics
nVidia Web Drivers

# Things to know about kexts after installing
KextBeast + Kext Utility

# Post-Install: Audio
ALC892 Audio
Mirone AppleHDA Kext+HDAEnabler1 was the only solution

# Post-Install: USB 3.0
GenericUSBXHCI.kext
Karabiner

# Bonus: Spelunky
One of the few Windows-only games I wanted to try and get running on OS X is Spelunky. (If you haven't tried it, it's great fun.) This involved delving even deeper into some jargon and strange program interfaces. Here's what I did to get it working.
I mostly referenced this Reddit thread (https://www.reddit.com/r/spelunky/comments/1jzevu/new_spelunky_works_on_os_x_using_wineskin/) but with u/shamansir's comments. Since that thread is locked, here's my interpretation of what I actually had to do, with updated version numbers for 2016:
(Warning: this text is incomplete as of 3/22/16)
* Download Wineskin Winery 1.7. Wineskin Winery is sort of a command center for making "wrappers" (read: executables) which apply certain conditions to folders that contain Windows programs. Upon opening Wineskin Winery, update the Wrapper Version to the latest (mine is Wineskin-2.6.2.)
* Add WS9Wine1.9.2 -- Engine means "Select WS9Wine1.2.2 engine in the list and push Create Blank Wrapper button and name it somehow to recognise it easily in future (it will be the .app file).
* Make a new wrapper using the 1.9.2 engine  means "[from the Wrapper you've created] Go to 'Wineskin Advanced', [find 'steam' and 'd3dx9' in the list] and push 'Install Software' button'".
* In winetricks, d3dx9
* Some say they can install Steam using winetricks but that did not work for me. What also didn't quite work was downloading the latest version of Steam for Mac and running it to install. It got stuck in an endless cycle of updating. At this point, I actually copied the entire Steam directory (minus steamapps) from my Windows PC and copied it all into the Steam folder. At this point, it actually successfully opens.
* Make sure you have the xbox360ce drivers in the same folder as Wineskin. You can always get back to this by right-clicking your wrapper and 'Open Package Contents.' I had to muck about with the buttons for my Xbox One controller. I've figured out the buttons:
* You MIGHT have to install the Xbox One driver for your Mac in general for the controller to work at all (link) I have encountered a bug where if I unplug my controller when OS X is running, it can crash my system instantly. This also happens on a legit Macbook Pro in my experience.
* Install Spelunky through Steam and it should work!
* You can then set the Wrapper to launch Spelunky directly using a path like:
* Add a nifty Spelunky icon to replace the Wineskin one!
* Caveats: It takes a bit longer to open because when you launch the program Steam has to start from scratch and checks for updates. Spelunky also always says it's installing DirectX drivers for the first time, but then it just jumps right into the program and I don't have to deal with Steam bugging out as it randomly seems to running this way. The whole process takes 22 seconds for me. The game runs amazingly well. Upon exiting, it might require an Alt+Tab to get out of a black screen and a force quit to truly end it, but I've had no issues result from this.
* ENJOY! Try this out with other games ... I have no idea what else works with this method.

# Persisting Issues
USB 3.0 is a little sketchy. I have an external hard drive attached to one that seems to eject every time the computer goes into sleep. (The good news is, sleeping works!)
