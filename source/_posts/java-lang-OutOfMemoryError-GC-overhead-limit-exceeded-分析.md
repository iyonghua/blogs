title: 'java.lang.OutOfMemoryError: GC overhead limit exceeded 分析'
author: weiyonghua
tags:
  - 生产定位
categories:
  - ''
  - FAQ
date: 2019-12-31 10:28:00
---
内存溢出问题定位分析：

step1：使用top命令查看进程号

![](https://oscimg.oschina.net/oscnet/up-36efe77d4addeedfcf35790c0d18b937615.png)

step2:使用jmap工具生成内存快照jmap -dump:format=b,file=tomcat.bin **17114**

step3:使用sz tomcat.bin 命令下载快照文件

step4：使用MemoryAnalyzer.exe工具分析内存使用

![](https://oscimg.oschina.net/oscnet/up-9f4a88b6a80291ef90a801fa3dc1ba4600b.png)![](https://oscimg.oschina.net/oscnet/up-83a548a82d5da3a74f0871ba83f503275b2.png)