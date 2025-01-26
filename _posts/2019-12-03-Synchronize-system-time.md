---
layout: post
title: Synchronize system time
categories: [System]
Description: How to synchronize system time
keywords: System Time
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

Description: How to synchronize system time.

## Quick start

### System 

CentOS 7

### Install NTP server
    
`$ yum -y install ntp or yaourt -S ntp`

### Synchronize time with Aliyun

`$ ntpdate time1.aliyun.com`

### OR

```shell
$ sudo timedatectl set-local-rtc 1 --adjust-system-clock
$ sudo timedatectl set-ntp 0
```

