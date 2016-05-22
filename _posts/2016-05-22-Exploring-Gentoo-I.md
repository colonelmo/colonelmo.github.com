---
layout: post
title: Exploring Gentoo, Part I: Installation
---

-----

I was gonna install and have a look at [gentoo linux](https://gentoo.org/) in my first longer-than-20-minutes window of free time, And here it is!
Gentoo is a meta distribution directed at letting the users have their own choices + experiencing great performance. This somehow yields the somehow famous gentoo saying: __If it moves, compile it!__

I'm gonna install this on a Dell optiplex 780 Dell SFF (Small Form Factor) which comes with an intel core2duo and 8GBs of ram, which means I'm gonna install the 64 bit version following [this](https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Installation) guide.

I ```dd```'d (wow) the iso file (install-amd64-minimal-20160519.iso) to a usb drive after verifying the signature and checksum of the image. There are gentoo, gentoo-nofb, and memset86 kernels available. gentoo-nofb disables framebuffer support. Framebuffer is a hardware independent graphics abstraction level which provides a standard and high(er) level interface to the graphics devices.

I booted ```gentoo``` with no extra parameters and the module auto-detection has done it's job. The network connection is working fine with the configurations from dhcp.

I went on with the tutorial and added ```-march=native``` to ```CFLAGS```. I chose the kde profile since I am not going to use this pc a lot AND I wanted to give kde a shot. I also didn't choose systemd over OpenRC but I will probably go with ```gnome/systemd``` if I ever plan to install gentoo on a laptop or something that I'll be using more often than this one. The kernel? I went with the default ```sys-kernel/gentoo-sources``` for now.

After some reading, I'm gonna go on with ```make menuconfig```.
I compiled the kernel and after generating initramfs image and fstab and a bunch of more steps, I'm now installing cronie as the system's default cron daemon.

I am installing grub now.

Aaaaaaaaaand it's finished. I messed up the installation :-/ I'll try again soon enough.

