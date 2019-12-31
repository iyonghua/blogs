title: mybatis日期字段映射成时间戳问题
author: weiyonghua
tags:
  - mybatis
categories:
  - 后端
date: 2019-12-31 16:12:00
---
解决方法：

1. 定义java对象为字符串，但存在一个问题。解析的字符串格式精确到毫秒，如2018-10-25 19:21:11.5。
2. 可以在java对象字段的getter方法上加jackson的注解： 
    @JsonFormat(pattern="yyyy-MM-dd HH:mm:ss",timezone = "GMT+8")