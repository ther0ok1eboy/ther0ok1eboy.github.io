---
layout: post
title: 🛡️ Step-by-step recovery guide for system certificates on Arch Linux 🔰 
categories: [System]
Description: Recovery system CA
keywords: Linux CA
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

Description: Step-by-step recovery guide for `/etc/ssl/certs/ca-certificates.crt` on Arch Linux.

## 🚀 Quickstart

If you've accidentally deleted `/etc/ssl/certs/ca-certificates.crt`, you may encounter the following problems:

![alt text](/images/system-ca/swappy-20250418-085842.png) 

![alt text](/images/system-ca/swappy-20250418-085854.png) 

Follow these steps to restore it:

### 📦 Reinstall `ca-certificates-utils` and `ca-certificates` packages

These packages contain the necessary CA certificates and tools to generate the `.crt` file.

```shell
$ sudo pacman -S ca-certificates ca-certificates-utils 
 ``` 

### 🛠️ Manually update the CA certificate store

Run the following command to extract and regenerate the certificate bundle:

```shell
$ sudo update-ca-trust extract
```

### ✔️ Verify the file was created

Check if the `.crt` file has been generated:

```shell
$ ls -l /etc/ssl/certs/ca-certificates.crt
```
