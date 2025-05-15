---
layout: post
title: 🚨 Bypass WeChat Client Open 🕵️‍♂️
categories: [Pen-test]
Description: How to bypass Please open the link on the WeChat client
keywords: WeChat Applet
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

Description: How to bypass Please open the link on the WeChat client.

## Description

When you want to copy the WeChat Official Account link to open it with your browser, you may encounter the following error, so you can't use f12 for source code auditing and code debugging.

![alt text](/images/bypass-wx-client-open/swappy-20250507-1.png) 

## ⚡ Quickstart

- First open the  WeChat Official Account. Use a packet grabber like **yakit**, and get a working **User-Agent**.

- Open Firefox, press f12 to open Developer Tools, and click on the phone-shaped icon in the upper-right corner, **Responsive**，**Edit list**，**Add new device**.

![alt text](/images/bypass-wx-client-open/swappy-20250507-2.png) 

```text
Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.0.0 Safari/537.36 NetType/WIFI MicroMessenger/7.0.20.1781(0x6700143B) WindowsWechat(0x63090a13) XWEB/11581 Flue
```

- Then select the new device you just added in the **Responsive** dropdown menu.

### 🔐 If you don't need WeChat authentication, you're done here, but generally you need WeChat authentication, and you'll run into the following situation.

![alt text](/images/bypass-wx-client-open/swappy-20250507-3.png) 

Here's an example of how I've solved this problem.

- Here you can see that when you visit the homepage using the browser, it makes a 302 jump and then authenticates to WeChat, whereas when you visit it in the browser that comes with WeChat, it doesn't make a jump!

![alt text](/images/bypass-wx-client-open/swappy-20250507-4.png) 

- To access it from WeChat Official Account, the cookie looks like this.

![alt text](/images/bypass-wx-client-open/swappy-20250507-5.png) 

- To access in a browser, the cookie looks like this.

![alt text](/images/bypass-wx-client-open/swappy-20250507-6.png) 

- You can find that when you visit WeChat, there is an additional OID field in the cookie, so when you visit using a browser, the cookie also carries the OID field, so that you don't jump to WeChat's authentication interface.

- Use **yakit** comes with a plug-in to modify the cookie, add the OID field can be, so that the use of the browser will be able to open the WeChat Official Account of the normal connection.

![alt text](/images/bypass-wx-client-open/swappy-20250507-7.png) 

![alt text](/images/bypass-wx-client-open/swappy-20250507-8.png) 
