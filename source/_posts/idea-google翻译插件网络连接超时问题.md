title: idea google翻译插件网络连接超时问题
author: weiyonghua
tags:
  - idea
categories:
  - FAQ
date: 2019-12-31 16:08:00
---
![](https://oscimg.oschina.net/oscnet/ec0577a28271540880fed31c409ac325104.jpg)

idea翻译插件之前一直用的好好的，最近莫名不能使用了。总结下问题解决办法：

由于idea激活弹窗频繁弹出开启了本地proxy

**[1.国内用户默认是translate.google.cn](http://1.xn--translate-9k3oh59boy5an1kf74c5q5dkh4c.google.cn)**的，除非你设备的语言环境为非中文。

![](https://oscimg.oschina.net/oscnet/271167e45a3aa3ac37df1ab5c988b82363c.jpg)

2.屏蔽**translate.google.cn代理**

![](https://oscimg.oschina.net/oscnet/5bc0f110b83dc7e80e6bafe7728cec448fc.jpg)