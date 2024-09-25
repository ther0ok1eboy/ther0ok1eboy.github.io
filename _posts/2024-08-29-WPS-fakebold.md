---
layout: post
title: WPS Fakebold
categories: [Software]
Description: WPS bold font display exception problem under linux system.
keywords: WPS
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

Description: WPS bold font display exception problem under linux system.

### Version:

`wps-office-cn-12.1.0.17885-1`

> link: https://aur.archlinux.org/packages/wps-office-cn

### Problem description: 

When using wps to open Chinese documents, Chinese fonts will fail to display because of the effect of font bolding.

### solution:

Just install the two packages.

`paru -S wps-office-mui-zh-cn freetype2-wps`
