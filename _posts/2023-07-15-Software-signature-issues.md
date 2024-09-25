---
layout: post
title: Software signature issues
categories: [Software]
Description: How to resolve software signature issues
keywords: IP Rfkill
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

Description: How to resolve software signature issues.

## Problem description

When you update your system, your may get following errors:

```shell
error: xxx: signature from "xxx <xxx@xxx>" is unknown trust
:: File /var/cache/pacman/pkg/xxx is corrupted (invalid or corrupted package (PGP signature)).
Do you want to delete it? [Y/n]
```

## Solution

```shell
sudo rm -R /etc/pacman.d/gnupg/
sudo pacman-key --init
sudo pacman-key --populate archlinux
sudo pacman-key --populate archlinuxcn
sudo pacman-key --refresh-keys
```

