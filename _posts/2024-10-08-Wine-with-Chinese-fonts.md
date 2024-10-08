---
layout: post
title: How to install Chinese fonts on Wine
categories: [Software]
Description: How to install Chinese fonts on Wine
keywords: Wine Chinese-fonts
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

Description: How to install Chinese fonts on Wine

## Problem

### Chinese characters on Wine do not show normally

Note that characters not being displayed and being displayed as garbled are different symptoms. If you see garbled characters, please make sure that your system language is the same as the one you are running Wine in. If there are Chinese characters missing or displayed as boxes, please continue reading this article.

## Solution

To solve the problem of Chinese characters can not be displayed there are three ideas: font links, font replacement or directly provide the appropriate font. Among them, the "font link" program to solve the best results, I recommend using. The other two are for reference only.

### Font link

The method is very simple, just create a text file named `chn_font.reg` with the following contents:

```shell

REGEDIT4
 
[HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\FontLink\SystemLink]
"Lucida Sans Unicode"="wqy-microhei.ttc"
"Microsoft Sans Serif"="wqy-microhei.ttc"
"MS Sans Serif"="wqy-microhei.ttc"
"Tahoma"="wqy-microhei.ttc"
"Tahoma Bold"="wqy-microhei.ttc"
"SimSun"="wqy-microhei.ttc"
"Arial"="wqy-microhei.ttc"
"Arial Black"="wqy-microhei.ttc"
```

> The font file `wqy-microhei.ttc` must exist at your system font directory or it's subdirectories. (/usr/share/fonts) And if you want to use other fonts, only replace the file name in `chn_font.reg`.

Open the registry `wine regedit`, import the above registry file can be. Chinese should be displayed perfectly!

![](/images/wine-font/swappy-20241008-173016.png)

Since Wine's font rendering is still flawed, installing the appropriate Windows stock libraries can solve some rare problems that could not be solved in the previous step

Open the `$ winecfg` and add three library functions `gdiplus riched20 riched30` into Libraries.

![](/images/wine-font/swappy-20241008-172645.png)


