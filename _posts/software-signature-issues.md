---
layout: post
title: Software signature issues
date: 2023-07-15 
tags: linux   
---

### Problem description

```shell
error: xxx: signature from "xxx <xxx@xxx>" is unknown trust
:: File /var/cache/pacman/pkg/xxx is corrupted (invalid or corrupted package (PGP signature)).
Do you want to delete it? [Y/n]
```

### Solution

```shell
sudo rm -R /etc/pacman.d/gnupg/
sudo pacman-key --init
sudo pacman-key --populate archlinux
sudo pacman-key --populate archlinuxcn
sudo pacman-key --refresh-keys
```

