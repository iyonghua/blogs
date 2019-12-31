title: css动态计算元素高度
author: weiyonghua
tags:
  - css
categories:
  - 前端
date: 2019-12-31 15:56:00
---
    height: calc(100vh - 50px);

使用calc函数动态计算元素高度，100vh代表屏幕高度

    $('#content .tabpanel-nav-header').resize(function () {
       let marginTopHeight = (this).height();
       $('#content .container .sx-tabs-container').css('margin-top', marginTopHeight);
    })
    

 