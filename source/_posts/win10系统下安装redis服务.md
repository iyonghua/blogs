title: win10系统下安装redis服务
author: weiyonghua
tags:
  - redis
categories:
  - 环境部署
date: 2019-12-31 15:54:00
---
**下载地址：**[https://github.com/MSOpenTech/redis/releases](https://github.com/MSOpenTech/redis/releases)

Redis 支持 32 位和 64 位。这个需要根据你系统平台的实际情况选择，这里我们下载 **Redis-x64-xxx.zip**压缩包到 **D:\Development_Tools\Redis**。

安装完成后，**安装目录**下大概会有以下几个文件：

> *redis-server.exe：服务端程序，提供redis服务*
> 
> *redis-cli.exe: 客户端程序，通过它连接redis服务并进行操作*
> 
> *redis-check-dump.exe：本地数据库检查*
> 
> *redis-check-aof.exe：更新日志检查*
> 
> *redis-benchmark.exe：性能测试，用以模拟同时由N个客户端发送M个 SETs/GETs 查询 (类似于 Apache 的ab 工具).*
> 
> *redis.windows.conf: 配置文件，将redis作为普通软件使用的配置，命令行关闭则redis关闭*
> 
> *redis.windows-service.conf：配置文件，将redis作为系统服务的配置，用以区别开两种不同的使用方式*

查看及修改**redis配置文件**见**：[http://www.cnblogs.com/ningskyer/articles/5730611.html](http://www.cnblogs.com/ningskyer/articles/5730611.html)**

打开**cmd** 窗口 使用cd命令切换目录到 **D:\Development_Tools\Redis **运行 

    redis-server.exeredis.windows.conf

 ****注：可以把 redis 的路径加到系统的环境变量里，这样就省得再输路径了，后面的那个 redis.windows.conf 可以省略，如果省略，会启用默认的。***

输入之后，会显示如下界面：

***![](https://images2017.cnblogs.com/blog/1100186/201711/1100186-20171114112706718-1859364134.png)***

这时候另启一个cmd窗口，原来的不要关闭，不然就无法访问服务端了。

 

**安装服务**：

>     redis-server --service-install redis.windows.conf

OK，大功告成，看看本地的服务，是不是加入了Redis了。

**启动服务**（安装服务之后，Redis并没有启动）：

    redis-server --service-start

**停止服务**：

    redis-server --service-stop

**测试：**

切换到redis目录下运行

    redis-cli.exe -h 127.0.0.1 -p 6379

设置键值对 **set myKey abc**

取出键值对 **get myKey**

**![](https://images2017.cnblogs.com/blog/1100186/201711/1100186-20171114113027562-1011142611.png)**

 

**介绍一款好用的redis可视化工具：Redis Desktop Manager**

下载地址：[https://redisdesktop.com/download](https://redisdesktop.com/download)