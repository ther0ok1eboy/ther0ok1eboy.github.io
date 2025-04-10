---
layout: post
title: How to bypass website Burpsuite detection
categories: [Pen-test]
Description: How to bypass
keywords: Burpsuite
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

Description: In this guide, you will learn how to bypass website Burpsuite detection.

## Quickstart

Using proxy tools such as Burpsuite for traffic interception and analysis is a very common operation in penetration testing or security research. However, some websites block such behavior by detecting the presence of proxy tools, usually to prevent malicious activity or protect sensitive data. Bypassing such detection requires careful analysis of the site's detection mechanism and appropriate countermeasures.

### User-Agent detection

- Principle of Detection: Some websites check the `User-Agent` field in the request header to determine if it is coming from a browser or other tool (such as Burpsuite).

- Bypass Method:
  - Modify the `User-Agent` in Burp Suite to make it look like a real browser.
  - Go to **Proxy -> Options** in Burp Suite and find the **Match and Replace** function.
  - Add a rule to replace the default Burp User-Agent with a common browser User-Agent, for example:
         ```shell
         User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/134.0.0.0 Safari/537.36
         ``
![pic](/images/bypass-bp-check/swappy-20250410-092404.png) 

### 2. HTTPS Traffic Characterization

- Principle of detection: Some websites inspect the certificate chain of HTTPS traffic to determine if it is signed by Burp Suite's self-signed certificates.

- Bypass Methods:
  - Install Burp CA certificate:
       - Import Burp Suite's CA certificate into the target device's trusted certificate store (e.g., browser or operating system).
       - For mobile devices, the certificate can be downloaded and installed via Burp Suite's `http://burp` page.
  - Dynamically Generated Certificates:
       - Ensure that `Generate CA-signed per-host certificates` is enabled in Burp Suite's **Proxy -> Options** to avoid the use of unified CA certificates.
  - Certificate Forgery:
       - If the target site has strict checks on the certificate chain, you can try to forge the certificate chain of the target site, but this requires a higher technical threshold.

### IP Address or Port Detection

- Principle of detection: Some websites check if the client's connection is coming from a specific proxy port (e.g. 8080, 8081) or block known proxy IPs through blacklisting mechanism.

- Bypass Methods:
  - Modify the listening port of Burp Suite:
    - In **Proxy -> Options** of Burp Suite, change the default listening port (e.g. from 8080 to 9090).
  - Use another proxy tool:
    - Switch to other proxies (such as mitmproxy or Charles Proxy), which may not be recognized by the target website.
  - Use a local SOCKS proxy:
    - Forward traffic through a locally running SOCKS proxy (such as an SSH tunnel) to avoid directly exposing the Burp Suite's ports.

### JavaScript Detection

- Principle of Detection: Some websites check for the presence of proxy tool features, such as Burp Suite's default request headers or unusual DOM behavior, via JavaScript on the front-end.

- Bypass Methods:
  - **Disable JavaScript detection scripts**:
    - Use a browser plugin (such as Tampermonkey) to write scripts that remove or modify the detection code.
  - **Simulates normal browser behavior**:
    - Enable browser extensions (e.g. FoxyProxy for Chrome) in Burp Suite to ensure traffic behavior is closer to real users.
  - **Analyze and replay legitimate requests**:
    - Use Burp Suite's **Repeater** or **Intruder** tools to manually replay legitimate requests to avoid triggering JavaScript detection.

### Time Delay Detection

- Principle of detection: Certain websites measure the response time of a request, and if an abnormal delay is found (e.g., time difference due to proxy processing), it may be determined that a proxy tool is being used.

- Bypass Methods:
  - **Optimize network environment**:
    - Ensure that the network latency between Burp Suite and the target server is as low as possible.
  - **Adjust Burp Suite settings**:
    - In **Proxy -> Options**, reduce the request processing time overhead.

### TLS Feature Detection

- Principle of detection: Some websites with high security will analyze the features (e.g., encryption suite, protocol version) during the TLS handshake to determine if they are consistent with standard browsers.

- Bypass Methods:
  - **Configure Burp Suite's TLS settings**:
    - In Burp Suite's **Project Options -> SSL/TLS**, select the cipher suite and protocol version that is compatible with the target website.
  - **Use advanced features of man-in-the-middle tools**:
    - If the target site has strict requirements for TLS features, try using more advanced man-in-the-middle tools (such as mitmproxy) that support more flexible TLS configurations.

