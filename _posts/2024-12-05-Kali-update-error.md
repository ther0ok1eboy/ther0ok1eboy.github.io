---
layout: post
title: Signature problems when upgrading a system with kali
categories: [System]
Description: Signature problems when upgrading a system with kali
keywords: Kali Signature Update
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

Description: Signature problems when upgrading a system with kali.

## Description

```shell
hacker@kali:~$ sudo apt update
Get:1 http://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling InRelease [41.5 kB]
Get:2 http://mirrors.aliyun.com/kali kali-rolling InRelease [41.5 kB]
Err:2 http://mirrors.aliyun.com/kali kali-rolling InRelease        
  The following signatures were invalid: EXPKEYSIG ED444FF07D8D0BF6 Kali Linux Repository <devel@kali.org>
Err:1 http://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling InRelease
  The following signatures were invalid: EXPKEYSIG ED444FF07D8D0BF6 Kali Linux Repository <devel@kali.org>
Reading package lists... Done
W: GPG error: http://mirrors.aliyun.com/kali kali-rolling InRelease: The following signatures were invalid: EXPKEYSIG ED444FF07D8D0BF6 Kali Linux Repository <devel@kali.org>
E: The repository 'http://mirrors.aliyun.com/kali kali-rolling InRelease' is not signed.
N: Updating from such a repository can't be done securely, and is therefore disabled by default.
N: See apt-secure(8) manpage for repository creation and user configuration details.
W: GPG error: http://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling InRelease: The following signatures were invalid: EXPKEYSIG ED444FF07D8D0BF6 Kali Linux Repository <devel@kali.org>
E: The repository 'http://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling InRelease' is not signed.
N: Updating from such a repository can't be done securely, and is therefore disabled by default.
N: See apt-secure(8) manpage for repository creation and user configuration details.
```

## Solution

```shell
wget archive.kali.org/archive-key.asc   
apt-key add archive-key.asc   
```
