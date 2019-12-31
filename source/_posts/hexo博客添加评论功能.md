title: hexo博客添加评论功能
author: weiyonghua
tags:
  - hexo
categories: []
date: 2019-12-31 18:14:00
---
在博客中加入了评论功能，发现网上以前的一些方法已经不适用，所以就综合了一下网上的教程，记录一下实现过程。

## 一、添加留言本 page

进入到博客的根目录，运行命令：

    hexo new page guestbook
    

## 二、留言本页面中添加多说访客代码

上一步中使用 hexo 命令新建了一个 page，进入到博客的source目录，里面会多了一个gusetbook文件夹，里面有一个index.md文件，打开该文件编辑：

    <divclass="ds-recent-visitors"data-num-items="28"data-avatar-size="42"id="ds-recent-visitors"></div>

这段代码加到index.md底部就行。

## 三、菜单设置中添加留言本

找到Next主题设置的_config.yml文件里面的menu项

    menu:
      home: /
      #about: /about
      archives: /archives
      tags: /tags
      categories: /categories
      guestbook: /guestbook
    

## 四、添加多语言文件的值

因为这里使用的是中文，找到languages文件夹里面的zh-CH.yml文件，menu子项中添加留言：

    menu:
      home: 首页
      archives: 归档
      categories: 分类
      tags: 标签
      about: 关于
      search: 搜索
      commonweal: 公益404
      guestbook: 留言
    

## 五、设置一个评论系统

我用的是Next 主题，本身就已经集成了valine，直接配置即可

下面网上搜来的其余系统,请自行搜索教程

- 多说
- 网易云跟帖
- 畅言
- 来必力（LiveRe）
- Disqus
- Hypercomments
- valine

## 开启Valine

# 注册Leancloud

我们的评论系统其实是放在Leancloud上的，因此首先需要去注册一个账号 [点我注册](https://links.jianshu.com/go?to=https%3A%2F%2Fleancloud.cn%2F)

1、 注册完以后需要创建一个应用，名字可以随便起，然后 进入应用->设置->应用key ,获取你的 `appid` 和 `appkey`

2、 打开主题配置文件 搜索 valine，填入appid 和 appkey

![](//upload-images.jianshu.io/upload_images/8921320-9201b23ae78846cf.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

配置appid 和 appkey.png

2、在Leancloud -> 设置 -> 安全中心 -> Web 安全域名 把你的域名加进去

🎉结束撒花～