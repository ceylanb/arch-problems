# Archlinux Problems and Solutions

This repository has the problems that I came across about Archlinux, and the solutions.

My laptop: Thinkpad E490  
GPU: Radeon

## Connecting to Windows using RDP

The following command can be used to connect a Windows using RDP. 

`xfreerdp +clipboard /u:HP /v:redacted-ip /dynamic-resolution +toggle-fullscreen`

`Ctrl+Alt+Enter` : Toggle fullscreen 


## Screen backlight keys will not work!

Here is the my case from Archlinux Wiki Page: (My video card is intel btw)

> Depending on the video card installed, sometimes xbacklight from xorg-xbacklight returns the message "No outputs have backlight property".  

Firstly, I tried the solution mentioned in Wiki. I created a new file `/etc/X11/xorg.conf.d/20-intel.conf` and added the following snippet to it.

```
Section "Device"
    Identifier  "Device0" 
    Driver      "intel"
    Option      "Backlight"  "intel_backlight"
EndSection
```

I rebooted the system and the Xorg was crashed as the wiki said. Then I checked the Intel Fastboot parameter, it was not active. 

> If you have enabled Intel Fastboot you might also get the No outputs have backlight property error. In this case, trying the above method may cause Xorg to crash on start up. You should disable it to fix the issue. It is known to cause issues with brightness control.

After some research, I realized that `xf86-video-intel` package is not installed. Installed it. Then I add some extra lines to  `/etc/X11/xorg.conf.d/20-intel.conf`, here is the final state:

```
Section "Device"
    Identifier  "Device0" 
    Driver      "intel"
    Option      "Backlight"  "intel_backlight"
EndSection

Section "Monitor"
    Identifier      "Monitor0"
EndSection

Section "Screen"
    Identifier      "Screen"
    Monitor         "Monitor0"
    Device          "Device0"
EndSection
```

I rebooted the system again. It worked! I can use the screen backlight control keys on my keyboard.
