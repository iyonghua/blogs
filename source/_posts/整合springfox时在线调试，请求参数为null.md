title: 整合springfox时在线调试，请求参数为null
author: weiyonghua
tags:
  - springfox
categories:
  - 后端
date: 2019-12-31 16:29:00
---
**问题描述：**

![](https://static.oschina.net/uploads/space/2016/1226/175719_f3i5_2485283.png)

* swagger在线调试时，一直获取不到查询参数的值。username一直为null*

 

排查结果：

    "parameters": [
              {
                "in": "body",
                "name": "username",
                "description": "username",
                "required": false,
                "schema": {
                  "type": "string"
                }
              },

这里原返回json字符串，in 的value是body，实际应该是query

![](https://static.oschina.net/uploads/space/2016/1226/175843_RNhx_2485283.png)

**解决方法：**

    public ReturnJsonBean login(@RequestParam(value = "username") String username, @RequestParam(value = "password") String password) {
    		returnthis.userService.doLogin(username, password);
    	}

原写法没有加@RequestParam注解，新增注解后完美解决！

    @RequestParam(value = "username")

 