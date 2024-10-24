---
layout: post
title: How to use Git
categories: [Git]
Description: How to use Git
keywords: Git
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

Description: How to use Git silky.

## Quickstart

+ 看Git的使用方法.

    `$ git --help`

+ 当前文件夹变为一个git仓库,创建git仓库.

    `$ git init`

+ 看当前仓库文件变化情况.

    `$ git status`

+ 增加修改.

    `$ git add # 可使用git add . 来添加当前仓库所有修改`

+ 次还没有提交的更改.

    `$ git diff # 比较工作区与暂存区的区别`

+ 回滚，撤销提交操作.

    `$ git reset`

+ Git提交自己身份(name).

    `$ git config --global user.name "USER_NAME"`

+ Git提交自己身份(email).

    `$ git config --global user.email "USER_EMAIL"`

+ Git提交内容.

    `$ git commit -m "COMMIT" # COMMIT为对提交的内容进行描述`

+ 让Git不提交某些文件/忽略某些文件.
    
    创建文件`.gitignore`并在文件中添加文件名/文件夹名即可.

+ 让Git不再追踪某个/某些文件.

    `$ git rm --cached FILE_NAME`

+ Git添加分支.

    `$ git branch BRANCH_NAME`

+ Git切换分支.

    `$ git checkout BRANCH_NAME`

+ 合并分支.

    `$ git merge BRANCH_NAME`

+ 列出本地分支.

    `$ git branch`

+ 删除分支.

    `$ git branch -d BRANCH_NAME # -D BRANCH_NAME 强制删除`

+ 添加远程仓库.

    `$ git remote add origin REMOTE_URL # REMOTE_URL 为远程地址`

+ 设置本地分支追踪远程分支.

    `$ git push --set-upstream`

+ 克隆仓库.

    `$ git clone REMOTE_URL`

+ 删除本地仓库.

    `$ rm -rf GIT_DIR/.git`


## Some problems when i use Git

### Repeat input password when your push git code

If we `$ git clone` to download by https instead of git@git(ssh) or `$ git push/pull`, git always hint us to input account/password, which make us disgust

+ Solution

    `$ git bash` into your local repository and input

    `$ git config --global credential.helper store`

    after execute above commands, it is auto product a txt file in which record your account and password.

    `$ git push` input the last password.

+ Latest solution

    [My new post](https://ther0ok1eboy.github.io/2024/09/24/Associate-remote-repository/)

### push error

+ Description.

    Git refuses to update, because of not exist local commit included by remote repository.

+ Why?

    The repository you want to push was been used(remote add origin master..) by other local repository and see 'Git push --help to find more information'.

+ Solution.

    Force to push: `$ git push -u origin +master`.

    Download your remote repository to modify then commit. `$ git clone [your remoterepositiry page]`.

### Error: pathspec 'modify' did not match any file(s) known to git

+ Solution.

    `$ git commit -a -m "modify"`.
