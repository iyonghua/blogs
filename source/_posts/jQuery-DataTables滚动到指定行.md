title: ' jQuery DataTables滚动到指定行'
author: weiyonghua
tags:
  - datatables
categories:
  - 前端
date: 2019-12-31 15:57:00
---
先上代码： 

    let scrollBody = $('#sku_table').parents('.dataTables_scrollBody');
    let index = $("#" + toId).parents('tr').index();
    let firstIndexTop = $('#sku_table').find('tbody tr:eq(0)').offset().top;
    let searchIndexTop = $('#sku_table').find('tbody tr:eq('+index+')').offset().top;
    scrollBody.scrollTop(searchIndexTop - firstIndexTop);

下面解释下用法

1.使用$('#sku_table').scrollTop调试了好久发现一直没有实现表格滚动条跳转。后来发现是jQuery datatables插件在表格的外层包装了一层div，这个滚动条是div的。

2.所以先选中div元素scrollBody，然后计算下表格第一行和要跳转的行的top高度

3.使用scrollBody.scrollTop跳转到指定的高度 searchIndexTop - firstIndexTop

 