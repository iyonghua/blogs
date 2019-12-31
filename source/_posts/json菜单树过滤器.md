title: json菜单树过滤器
author: weiyonghua
tags:
  - json
categories:
  - 前端
date: 2019-12-31 15:59:00
---
递归实现json菜单树过滤，过滤到匹配节点并保留其父节点

    //递归过滤节点functionnodeFilter(menuList, fiterName) {
                 var reg = newRegExp(fiterName);
                  var retMenuList = newArray();
                 if (menuList && menuList.length > 0) {
                     for (var c of menuList) {
                         if (c.subModules) {
                             c.subModules = nodeFilter(c.subModules, fiterName);
                         }
                         if (c.thirdModules) {
                             c.thirdModules = nodeFilter(c.thirdModules, fiterName);
                         }
                         if ((c.subModules && c.subModules.length > 0)
                             || (c.thirdModules && c.thirdModules.length > 0)
                             || isMatch(c, reg)) {
                              retMenuList.push(c);
                         }
                     }
                 }
                 return retMenuList;
             }
            
            
             functionisMatch(menu, reg) {
                 return menu.heading.match(reg)
                     || pinyin.getFullChars(menu.heading).toLowerCase().match(reg)
                     || pinyin.getCamelChars(menu.heading).toLowerCase().match(reg);
             }

 