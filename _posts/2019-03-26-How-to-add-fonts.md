---
layout: post
title: How to add new fonts
categories: [Software]
Description: How to add new fonts
keywords: fonts
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

Description: how to add some fonts on your system.

## Quickstart

+ Create a fonts directory.

    ```shell
    $ cd /usr/share/fonts
    $ mkdir new-fonts
    $ cd new-fonts
    ```

+ Download fonts style on [Nerd Font](https://www.nerdfonts.com/) and extract it.

+ Copy the file into this new-fonts directory.

    After your extract the fonts packages you downloaded, you will get some file.
    
    ![png](/images/add-fonts/swappy-20241024-151803.png)
    
    ```shell
    $ cp *.otf /usr/share/fonts/new-fonts
    # Or copy all directory, like this
    $ cp Nerd-Font/ /usr/share/fonts/
    ```
    ``

+ Load the fonts and fresh font cache.

    `$ fc-cache -fv (yum install fontconfig)`

+ List all fonts on your system.
    
    `$ fc-list`
