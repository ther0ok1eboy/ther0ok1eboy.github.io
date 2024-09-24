---
layout: post
title: Wireless Configuration
date:  2024-02-10
tags: network
---

## Check the driver status

`$ lspci -k | grep WIFI`

Also check the output of the ip link command to see if a wireless interface was created; usually the naming of the wireless network interfaces starts with the letters "wl", e.g. wlan0 or wlp2s0. Then bring the interface up with:

`$ ip link set INTERFACE up`

> Note: 
- If you get errors like RTNETLINK answers: `Operation not possible due to RF-kill`, make sure that the device is not hard-blocked or soft-blocked.

- If you get the error message SIOCSIFFLAGS: No such file or directory, it most certainly means that your wireless chipset requires a firmware to function.

## Rfkill caveat

Many laptops have a hardware button (or switch) to turn off the wireless card; however, the card can also be blocked by the kernel. This can be handled by rfkill(8). To show the current status:

`$ rfkill`

If the card is hard-blocked, use the hardware button (switch) to unblock it. If the card is not hard-blocked but soft-blocked, use the following command:

`rfkill unblock INTERFACE`
