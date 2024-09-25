---
layout: post
title: How to associate remote repository
categories: [Git]
Description: 2024 latest solutions for associate remote repositories.
keywords: git
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

Description: 2024 latest solutions for associate remote repositories.

## Solution

Transfers between local Git repositories and GitHub repositories are encrypted over SSH, so the solution is  configure the SSH key.

## Step

### Generate a key pair in the local ssh directory
`$ ssh-keygen -t rsa -C "account@gmail.com"`. this email is your Github account, and you will get two file: `id_rsa` and `id_rsa.pub` in `$HOME/.ssh` directory.

### Configure the SSH key in Github
- Login your Github account and click the `Settings` under account avatar(https://github.com/settings/profile). 
- Click `SSH and GPG keys`. and click `New SSH key`. 
- Fill in the text box with the public key(id_rsa.pub) you just generated.

### Testing connectivity
`$ ssh -T git@github.com`. if show message `Hi ther0ok1eboy! You've successfully authenticated, but GitHub does not provide shell access.` after your execute the command, congratulations! the configuration is completed.

### Initialise a local repository and associate it with a remote repository
- `$ git init` Initialise a local repository and execute `$ git add . && git commit` command. 
- Then execute the following command `$git remote add origin git@github.com:YOUR_REMOTE_REPOSITORY`. eg: git remote add origin git@github.com:ther0ok1eboy/test. 
- After that, you can push your code normally using command `$git push -u origin master`.
