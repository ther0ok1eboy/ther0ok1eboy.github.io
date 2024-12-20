---
layout: post
title: Usage of compression tools
categories: [command]
Description: This article will introduce the usage of common compression tools
keywords: Zip rar tar 7z
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

Description: This article will introduce the usage of common compression tools.

## Zip and unzip

### Some common parameters

| options             | explanation                                                                                          |
|---------------------|------------------------------------------------------------------------------------------------------|
| -d<store directory> | unzip to custom directory.                                                                           |
| -D                  | zip the file without directory.                                                                      |
| -F                  | try to fix broken file.                                                                              |
| -g                  | compress directory including have been compressed directory instead of create a new compressed file. |
| -m                  | after compressing, delete sourse file.                                                               |
| -q                  | not show the process of compressing.                                                                 |
| -r                  | recursion.                                                                                           |
| -S                  | compress directory including conceal files.                                                          |
| -O                  | specify a character encoding for DOS, Windows and OS/2 archives                                      |

### Common usage

`$ zip -r -q zip-name $HOME/[your directory]` 

`$ zip -r -q zip-name *` if this file at your directory

`$ unzip zip-packages` unzip all files without directory (not recommend).

`$ unzip zip-packages -d my-dirctory` unzip all files into a directory named my-directory.

`$ unzip zip-packages -O GBK zip-name` specify the GBK encoding to extract, the default encoding is utf-8.

## Unrar and rar

### Some common parameters

| options | explanation                      |
|---------|----------------------------------|
| -e      | extract files without dirctory   |
| -p      | print extract infomation         |
| -x      | extract files with complete path |

### Common use

`$ rar a c.rar *.c` compress all of C file under the current directory into a package named c.rar.

`$ unrar x c.rar` extract with full paths. 

## Tar

### Some common parameters

| options | explanation                                       |
|---------|---------------------------------------------------|
| -c      | create new Archive                                |
| -x      | common compress                                   |
| -z      | compress with gzip                                |
| -v      | show compress process                             |
| -f      | use Archive name and it the last parameter        |
| -r      | compress file into a tail of exiting Archive file |

### Common use

`$ tar -cf c.tar *.c` compress all of C file under the current directory into a Archive named c.tar.

`$ tar -rf c.tar *.cpp` add all of CPP file under the current directory into a tail of exiting Archive file. 

`$ tar -czf c.tar.gz *.py` after compress an Archive file named c.tar, use `gzip` to compress this file named c.tar.gz. 

### Some conclusions

| command          | easy usage        |
|------------------|-------------------|
| *.tar            | tar -xvf          |
| *.gz             | gzip -d & gunzip  |
| *.tar.gz & *.tgz | tar -xzf          |
| *.bz2            | bzip2 d & bunzip2 |
| *.tar.bz2        | tar -xjf          |
| *.rar            | unrar x           |

## 7za

### Comman use

`$ 7za x *.7z` extract files with full path.
