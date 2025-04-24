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

+ View how to use Git.

    `$ git --help`

+ Initialize the current folder as a Git repository.

    `$ git init`

+ View the status of the files in the current repository.

    `$ git status`

+ Add changes.

    `$ git add # You can use git add . to add all changes in the current repo`

+ View uncommitted changes.

    `$ git diff # Compare working directory with staging area`

+ Rollback or undo commit actions.

    `$ git reset`

+ Set Git identity (name).

    `$ git config --global user.name "USER_NAME"`

+ Set Git identity (email).

    `$ git config --global user.email "USER_EMAIL"`

+ Commit changes.

    `$ git commit -m "COMMIT" # COMMIT is the message describing the commit`

+ Tell Git to ignore some files/folders.

    Create a file named `.gitignore` and add filenames/folder names to it.

+ Stop tracking a specific file.

    `$ git rm --cached FILE_NAME`

+ Create a new Git branch.

    `$ git branch BRANCH_NAME`

+ Switch Git branches.

    `$ git checkout BRANCH_NAME`

+ Merge branches.

    `$ git merge BRANCH_NAME`

+ List local branches.

    `$ git branch`

+ Delete a branch.

    `$ git branch -d BRANCH_NAME # -D BRANCH_NAME force deletes`

+ Add a remote repository.

    `$ git remote add origin REMOTE_URL # REMOTE_URL is the remote address`

+ Set local branch to track a remote branch.

    `$ git push --set-upstream`

+ Clone a repository.

    `$ git clone REMOTE_URL`

+ Delete the local repository.

    `$ rm -rf GIT_DIR/.git`


## Some problems when I use Git

### Repeat input password when your push git code

If we `$ git clone` using https instead of git@git(ssh) or `$ git push/pull`, Git always prompts us to input username/password, which is annoying.

+ Solution

    `$ git bash` into your local repository and input

    `$ git config --global credential.helper store`

    After executing the above command, Git will automatically create a txt file storing your account and password.

    `$ git push` one last time with your credentials.

+ Latest solution

    [My new post](https://ther0ok1eboy.github.io/2024/09/24/Associate-remote-repository/)

### Push error

+ Description.

    Git refuses to update, because your local commit is not included in the remote repository.

+ Why?

    The repository you want to push to was already used (remote add origin master..) by another local repository. See 'Git push --help' for more info.

+ Solution.

    Force push: `$ git push -u origin +master`.

    Or clone the remote repository again: `$ git clone [your remote repository URL]`.

### Error: pathspec 'modify' did not match any file(s) known to git

+ Solution.

    `$ git commit -a -m "modify"`.
