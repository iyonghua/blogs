title: 调整tomcat vm内存大小
author: weiyonghua
tags: []
categories: []
date: 2017-03-13 14:17:00
---

**在tomcat vm启动参数中调整虚拟机内存大小：**

> -server -XX:PermSize=64M -XX:MaxPermSize=512m -Xms512m -Xmx1024m

![这里写图片描述](http://img.blog.csdn.net/20170421134047832?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VpeW9uZ2h1YTE2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20170421134119849?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VpeW9uZ2h1YTE2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)