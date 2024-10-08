---
layout: post
title: How to install VNC on server and client
categories: [Software]
Description: How to install VNC service on server and client
keywords: VNC
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

Description: How to install VNC service on server and client

## Introduction

If you are a college student who bears the boring lessons seating in the boring seat. And you desire play computer game but the computer is not surround you. In this time, you only need a VNC viewer installed on Android, which make you use your phone to manage your Linux desktop.

## Quick start

This is my system information:(Arch linux)

![](/images/vnc-on-server/swappy-20241007-185105.png)

### Service on your Linux desktop

+ Install VNC service

    `$ sudo pacman -S x11vnc`

+ Setting login password

    `$ x11vnc -storepasswd` and this password will store on `~/.vnc/passwd`  

+ Creating login log

    `$ touch ~/.vnc/x11vnc.log`

+ Startup VNC service

    `$ x11vnc -nap -wait 50 -noxdamage -rfbauth ~/.vnc/passwd -display :0 -nocursor -forever -o ~/.vnc/x11vnc.log -bg` (you all process information will store the path `~/.vnc/x11vnc`)

    Run above command you will get your server's listen port. (remember the port and default port is 5900)

+ Check if this port is listening

    `sudo netstat -lntp | grep x11vnc` if there is not this port, check the log file `cat ~/.vnc/x11vnc.log` to find more error details.  

### Client on Android

I recommend this app named VNC viewer and you can click [here](https://android-vnc-viewer.en.softonic.com/android) to download.

Input the IP address(must be on the same LAN) and correct port.

## End

![]()

