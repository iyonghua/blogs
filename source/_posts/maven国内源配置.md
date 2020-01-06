title: maven国内源配置
author: weiyonghua
tags:
  - maven
categories:
  - FAQ
date: 2020-01-06 17:46:00
---
> 新年开工后要开始新的项目，但是发现一些项目的依赖没有在阿里仓库Central或Public源之中，项目依赖还是要在外网中下载，很慢、很烦······
> 所以一番查找后，替换为阿里官方仓库服务站点：[https://maven.aliyun.com/mvn/...](https://maven.aliyun.com/mvn/view)

[阿里官方仓库]![2688363599-5c64d5dbcaeb1_articlex.png](http://ww1.sinaimg.cn/large/0082pGazgy1gamzj6wz4bj30m80613za.jpg)

### 具体操作

1. 打开** ${maven_home}/conf/settings.xml **
2. 
在**<mirrors>**上面配置

    <mirror><id>aliyun-public</id><mirrorOf>*</mirrorOf><name>aliyun public</name><url>https://maven.aliyun.com/repository/public</url></mirror><mirror><id>aliyun-central</id><mirrorOf>*</mirrorOf><name>aliyun central</name><url>https://maven.aliyun.com/repository/central</url></mirror><mirror><id>aliyun-spring</id><mirrorOf>*</mirrorOf><name>aliyun spring</name><url>https://maven.aliyun.com/repository/spring</url></mirror><mirror><id>aliyun-spring-plugin</id><mirrorOf>*</mirrorOf><name>aliyun spring-plugin</name><url>https://maven.aliyun.com/repository/spring-plugin</url></mirror><mirror><id>aliyun-apache-snapshots</id><mirrorOf>*</mirrorOf><name>aliyun apache-snapshots</name><url>https://maven.aliyun.com/repository/apache-snapshots</url></mirror><mirror><id>aliyun-google</id><mirrorOf>*</mirrorOf><name>aliyun google</name><url>https://maven.aliyun.com/repository/google</url></mirror><mirror><id>aliyun-gradle-plugin</id><mirrorOf>*</mirrorOf><name>aliyun gradle-plugin</name><url>https://maven.aliyun.com/repository/gradle-plugin</url></mirror><mirror><id>aliyun-jcenter</id><mirrorOf>*</mirrorOf><name>aliyun jcenter</name><url>https://maven.aliyun.com/repository/jcenter</url></mirror><mirror><id>aliyun-releases</id><mirrorOf>*</mirrorOf><name>aliyun releases</name><url>https://maven.aliyun.com/repository/releases</url></mirror><mirror><id>aliyun-snapshots</id><mirrorOf>*</mirrorOf><name>aliyun snapshots</name><url>https://maven.aliyun.com/repository/snapshots</url></mirror><mirror><id>aliyun-grails-core</id><mirrorOf>*</mirrorOf><name>aliyun grails-core</name><url>https://maven.aliyun.com/repository/grails-core</url></mirror><mirror><id>aliyun-mapr-public</id><mirrorOf>*</mirrorOf><name>aliyun mapr-public</name><url>https://maven.aliyun.com/repository/mapr-public</url></mirror>