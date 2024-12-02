---
layout: post
title: 地图大师API payload总结
categories: [Pen-test]
Description: 地图大师API payload总结
keywords: API key
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

Description: 地图大师API payload总结 By 地图大师returnwrong.

## Quickstart

高德地图api、百度地图api、腾讯地图api最终总结了以下这些payload，师傅们可以直接将key值或ak值（百度地图的密钥叫ak）直接代入我总结的payload进行验证：

```shell
高德webapi：
https://restapi.amap.com/v3/direction/walking?origin=116.434307,39.90909&destination=116.434446,39.90816&key=这里写key

高德jsapi：
https://restapi.amap.com/v3/geocode/regeo?key=这里写key&s=rsv3&location=116.434446,39.90816&callback=jsonp_258885_&platform=JS

高德小程序定位：
https://restapi.amap.com/v3/geocode/regeo?key=这里写key&location=117.19674%2C39.14784&extensions=all&s=rsx&platform=WXJS&appname=c589cf63f592ac13bcab35f8cd18f495&sdkversion=1.2.0&logversion=2.0

百度webapi：
 https://api.map.baidu.com/place/v2/search?query=ATM机&tag=银行&region=北京&output=json&ak=这里写key

百度webapiIOS版：
https://api.map.baidu.com/place/v2/search?query=ATM机&tag=银行&region=北京&output=json&ak=这里写key=iPhone7%2C2&mcode=com.didapinche.taxi&os=12.5.6

腾讯webapi：
https://apis.map.qq.com/ws/place/v1/search?keyword=酒店&boundary=nearby(39.908491,116.374328,1000)&key=这里写key
```

## Check

下面先给师傅们演示下可以成功打地图API泄露的站点，可以成功回显一些敏感信息，我们把我们收集到的key值或ak值（百度地图的密钥叫ak）放入我们上面总结的payload中，如下：

![](/images/map-api-key/swappy-20241202-131022.png)

错误的案例演示如下，如果提示类似于app调用失败，或者错误代码10006或10008就代表这个问题已经修复了，就不用再看了。我这里出现了10008说明，漏洞不存在。

![](/images/map-api-key/swappy-20241202-131406.png)

## Report template

```html
漏洞标题：xx网站-xx管理后台-存在xx地图api接管漏洞

漏洞描述：xx网站-生活圈管理后台-存在高德地图api接管漏洞，攻击者可利用抓取的高德地图ak值任意调用属于xx的高德地图的api额度造成XX的高德地图api额度被恶意盗用，消耗等。当额度被消耗完毕后，会造成地图加载异常，定位服务无法使用等，影响用户体验。

漏洞复现：
1、浏览器访问https://xxx.xxxx.com/

2、F12查看页面源代码搜索key获得key值：

3、由于此处XX管理员对于XX地图控制台配置错误，导致任意用户均可调用该api用以消耗额度，此处参考高德地图官方web调用接口构造payload：https://api.map.baidu.com/place/v2/search?query=ATM机&tag=银行&region=北京&output=json&ak=这里写key

调用成功如下图所示：
若配置正确攻击者调用该接口会提示错误代买10006或10008无法通过该接口获取地图信息

修复方案：
参考XX地图官方文档
```
