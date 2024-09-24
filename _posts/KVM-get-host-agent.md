---
layout: post
title: Virtual machine kali go hosting agent
date:  2024-02-26
tags: network
---

## KVM get host agent

> tips: need `proxychains4` and `clash for windows bin`

### install proxychains4 on KVM virtual machine

- `$ sudo apt install proxychains4`

### configurate the file of proxychains4

- file path: `/etc/proxychains4.conf`

- wipe out the `#` ahead of `dynamic_chain` and add the `#` ahead of `strict_chain`

- add your agent ip in last line: `socks5 172.16.101.187 7890`

### allow lan on agent program

- open the `Allow LAN` icon on Clash for windows bin

### how to use

- `proxychains4 YOUR COMMANDS`