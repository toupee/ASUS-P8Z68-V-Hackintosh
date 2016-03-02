# ASUS-P8Z68-V-Hackintosh
My notes on making this motherboard and parts Hackintoshable. So far, so good.

Thanks to many many people for getting me up and running, especially u/corpnewt (Reddit), u/macmeister (Reddit), DavidGoldman (Github), tkrotoff (Github), Mirone (Github), and so many others.

I found that there were very few guides that were especially helpful in undertaking this little project. Everything is made out to be so simple. Create a USB stick with Unibeast! Throw Multibeast on there! Boot 'er up! S/L/R! Kexts! DSDTs! Umm... figuring all this stuff out was actually a lot harder than I expected. There's still a ton I don't know. But hopefully this guide will help some folks with the same setup as me get up and running in a couple hours, rather than a couple dozen hours like it took me. (Weekend Warrior? More like... Week-long Warrior.)

This guide is for OS X 10.11 El Capitan.

# My setup

I think it's important that I share my build:
* ASUS P8Z68-V Motherboard (NOT PRO/GEN3 or LX)
* nVidia GTX 970
* 


# Motherboard Setup
BIOS Version
No patching necessary

# USB Stick Preparation
Unibeast
Hidden EFI Partition + How to mount
Kexts
cpus=1
Some USB sticks were simply borked

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

