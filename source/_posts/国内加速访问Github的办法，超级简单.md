title: 国内加速访问Github的办法，超级简单
author: weiyonghua
tags: []
categories:
  - 奇淫巧技
date: 2019-12-30 17:35:00
---
# 国内加速访问Github的办法，超级简单

[![扩展迷Extfans](https://pic2.zhimg.com/v2-013471d1e9c3de2c1f4a3b4ba1be5ba1_xs.jpg)](//www.zhihu.com/people/kuo-zhan-mi-extfans)

[扩展迷Extfans](//www.zhihu.com/people/kuo-zhan-mi-extfans)

发现有趣的网站，玩转Chrome扩展，尽在扩展迷Extfans

37 人赞同了该文章

自从GitHub私有库免费后，又涌入了一大批开发爱好者。

但国内访问GitHub的速度实在是慢得一匹，在clone仓库时甚至只有10k以下的速度，大大影响了程序员的交友效率。
![](https://pic4.zhimg.com/v2-dec4f419d4d8ba5ba1ed260eaebab4cb_b.jpg)![](https://pic4.zhimg.com/80/v2-dec4f419d4d8ba5ba1ed260eaebab4cb_hd.jpg)
GitHub在国内访问速度慢的问题原因有很多，但最直接和最主要的原因是GitHub的分发加速网络的域名遭到dns污染。

今天我们就介绍通过修改系统hosts文件的办法，绕过国内dns解析，直接访问GitHub的CDN节点，从而达到加速的目的。

不需要科（）学（）上网，也不需要开代理加速器。

一、打开[http://IPAddress.com](https://link.zhihu.com/?target=http%3A//IPAddress.com)网站，查询下面3个网址对应的IP地址

1. [http://github.com](https://link.zhihu.com/?target=http%3A//github.com)

2. [http://assets-cdn.github.com](https://link.zhihu.com/?target=http%3A//assets-cdn.github.com)

3. [http://github.global.ssl.fastly.net](https://link.zhihu.com/?target=http%3A//github.global.ssl.fastly.net)
![](https://pic4.zhimg.com/v2-002f06094e4bab7ef86fe0d6eb086883_b.jpg)![](https://pic4.zhimg.com/80/v2-002f06094e4bab7ef86fe0d6eb086883_hd.jpg)
二、修改本地电脑系统hosts文件

路径 C:\Windows\System32\drivers\etc

直接在最后加入以下代码（此时可能需要管理员权限,可以将hosts复制到桌面，修改好了再复制回去覆盖原先的）：

192.30.253.112 [http://github.com](https://link.zhihu.com/?target=http%3A//github.com)

151.101.184.133 [http://assets-cdn.github.com](https://link.zhihu.com/?target=http%3A//assets-cdn.github.com)

151.101.185.194 [http://github.global.ssl.fastly.net](https://link.zhihu.com/?target=http%3A//github.global.ssl.fastly.net)
![](https://pic1.zhimg.com/v2-a99ec919d7147d655465e68fc5e44054_b.jpg)
![](https://pic1.zhimg.com/80/v2-a99ec919d7147d655465e68fc5e44054_hd.jpg)
三、刷新系统dns缓存

用WIN+R快捷键打开运行窗口，输入命令：cmd并回车进入命令行窗口。

接着输入命令：ipconfig /flushdns回车后执行刷新本地dns缓存数据即可。
![](https://pic4.zhimg.com/v2-bbf2bd87a4ecc7a6f8b11a664a18e45b_b.jpg)
![](https://pic4.zhimg.com/80/v2-bbf2bd87a4ecc7a6f8b11a664a18e45b_hd.jpg)
内网限速的情况下效果如图所示，根据网上的普遍反馈来看，达到10m/s不是问题。
![](https://pic3.zhimg.com/v2-8c4537a8eb0a3afdf31d8893dee90d7a_b.jpg)![](https://pic3.zhimg.com/80/v2-8c4537a8eb0a3afdf31d8893dee90d7a_hd.jpg)