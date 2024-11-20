---
layout: post
title: How to install VMware workstation
categories: [Software]
Description: How to install VMware workstation
keywords: fonts
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

Description: How to install VMware workstation.

## Quick start

- Go to vmware's [website](https://www.vmware.com/) and download the latest bundle file.

- Installing dependencies
 
 `$ sudo pacman -S fuse2 gtkmm  pcsclite libcanberra`
 
- Install the kernel header

 `$ sudo pacman -S linux-headers and choose the suitable version($ uname -r)`
 
- Loading VMware's modules and reboot

 `$ sudo modprobe -a vmw_vmci vmmon or reboot`
 
> source from https://www.jianshu.com/p/de571b44ab60





