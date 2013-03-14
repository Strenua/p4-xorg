P4 Xorg
=======

This repo documents a WIP of getting Xorg to work (natively) on the Samsung Galaxy Tab.

Requirements
------------

* Rooted Galaxy Tab 10.1 (p4), though similar devices may share a similar process
* Android with kernel 3.1.10+
* A working Archlinux-ARM image, unless you intend to do a lot of compiling
* A functional brain (preferably humanoid)

Process
-------

This is a short version of the steps I took to get a functional Xorg. Insert annoying disclaimer statement here.

First off, install things.

```
pacman -Syu xorg-server xf86-video-fbdev mesa
```

And the touchscreen driver:

```
wget https://aur.archlinux.org/packages/xf/xf86-input-mtev-meego/xf86-input-mtev-meego.tar.gz
tar -xvf xf86-input-mtev-meego.tar.gz && cd xf86-input-mtev-meego
su -c 'makepkg --asroot -A && pacman -U xf86-input-mtev-meego*.tar.*'
```

Install the [xorg configuration](https://github.com/Strenua/p4-xorg/blob/master/xorg.conf) to /etc/X11/xorg.conf.

Last step for Arch, softlink the framebuffer character devices to /dev/ (from /dev/graphics/):
```
sudo ln -s /dev/graphics/fb* /dev/
```

Back in Android, prevent the Android display server from respawning, and then kill it:

```
mount -o rw,remount /system
chown 666 /system/bin/{surfaceflinger,app_process,bootanimation}
killall surfaceflinger zygote
```

Tada, you might be able to start X now. Hopefully.
Important: DO NOT REBOOT. Before you reboot or lose power or some such thing,

```
chmod 777 /system/bin/{surfaceflinger,app_process,bootanimation}
```
It's a good idea to make a script for this, and of course, have backup images on hand to flash.
If you want to run the Android UI again without rebooting, simply run the above, then `surfaceflinger`.
