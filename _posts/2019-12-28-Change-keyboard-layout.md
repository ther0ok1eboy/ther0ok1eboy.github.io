---
layout: post
title: How to change keyboard binding
categories: [Software]
Description: How to change keyboard binding
keywords: Xmodmap
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

Description: How to change keyboard binding.

## Introduction

I guest that you may get bored with your traditional keyboard binding and many usually key are not at these positions in which you can convenient to touch. Today i will share a powerful and easy-to-use application named xmodmap who is a part of family of Xorg and supports permanent modify your traditional keyboard binding, so you should install some dependence of Xorg. 

## Quick start

### Download some requirements

+ Download xorg.

    `$ sudo pacman -S xorg`

### Configure

+ Show your recent keybord layouts.

    `$ xmodmap`

+ Show id of your of all keybord layouts.

    `$ xev` when you press any keyboard you can see its id

+ Input your layouts into `.xmodmap` file.

    `$ xmodmap -pke > ~/.xmodmap && vim .xmodmap`

+ Change key you want.

    I recommend to change the Esc and CapLock because you will usually use key Esc when you edit your articles but the headache is the key Esc is far from your finger, which make you uncomfortable touch it.

    Change their contents behind the id and input 'clear lock' into header of file and 'add lock = Cap_Lock'

+ Save config file.

    `$ xmodmap ~/.xmodmap`

## Other way to remap Caps Lock to Escape key

https://ejmastnak.com/tutorials/arch/caps2esc/
