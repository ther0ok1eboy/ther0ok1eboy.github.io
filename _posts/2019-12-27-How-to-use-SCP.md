---
layout: post
title: How to use SCP command
categories: [command]
Description: How to use SCP command
keywords: ssh scp
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

Description: How to use SCP command.
    
## Quick start

### Upload a file to your server

    `$ scp (-P your ssh port) your file USER@your server ip:/root/myUploadFile` 

### Upload a dirctorty to your server

    `$ scp (-P your ssh port) -r /yuor directory USER@your server ip:/root/myDirectory`   

### Download file form your sever

    `$ scp (-P your ssh port) USER@your server ip :/the file your want` 

### Download directory form your server

    `$ scp (-P your ssh port) $USER@your server ip:/the directrory you want` <++>  
