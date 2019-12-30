title: hexo deploy发布失败解决办法
tags:
  - hexo
  - deploy
categories:
  - FAQ
author: weiyonghua
date: 2017-03-03 11:16:00
---
**问题描述：**
==ssh秘钥已添加，但没有权限提交==
```
Error: Host key verification failed.
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.

    at ChildProcess.<anonymous> (D:\WORKSPACES\itblog\node_modules\hexo-util\lib\spawn.js:37:17)
    at emitTwo (events.js:106:13)
    at ChildProcess.emit (events.js:191:7)
    at ChildProcess.cp.emit (D:\WORKSPACES\itblog\node_modules\cross-spawn\lib\enoent.js:40:29)
    at maybeClose (internal/child_process.js:877:16)
    at Process.ChildProcess._handle.onexit (internal/child_process.js:226:5)
FATAL Host key verification failed.
fatal: Could not read from remote repository.

```
**解决办法：**		
新增个人公钥（部署公钥）

1.如何生成ssh公钥
你可以按如下命令来生成sshkey: 	
```
ssh-keygen -t rsa -C "xxxxx@xxxxx.com"  

# Generating public/private rsa key pair...
# 三次回车即可生成 ssh key
```

查看你的public key，并把他添加到 Git @ OSC SSH key添加地址
```
cat ~/.ssh/id_rsa.pub
# ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC6eNtGpNGwstc....
```
添加后，在终端（Terminal）中输入
```
ssh -T git@git.oschina.net
```
若返回
```
Welcome to Git@OSC, yourname!
```
则证明添加成功。		

2.怎么添加用户ssh key?

 1. 点击右上角的输入图片说明标志,进入个人中心,然后点击左侧的ssh公钥后在下图位置填写你的ssh公钥
 2. 点击确定,然后验证密码(即你的注册账号密码)就完成了ssh公钥添加
![这里写图片描述](https://static.oschina.net/uploads/img/201610/18115822_miTO.png)


3.项目的ssh key和用户的ssh key两处地方有什么不同?

```
项目的sshkey只针对项目,且我们仅对项目提供了部署公钥,即项目下的公钥仅能拉取项目,这通常用于生产服务器拉取仓库的代码。
而用户的key则是针对用户的,用户添加了key就对用户名下的项目和用户参加了的项目具有权限,一般而言,用户的key具有推送和拉取的权限,而项目的key则只具有拉取权限
```