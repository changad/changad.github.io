---
layout: post
title:  "给jQuery DOM操作生成元素绑定事件"
date:   2017-01-16 14:47:13 +0000
categories: xiaochang update
---



DOM操作,往父级元素中添加元素

$("#zuowei").append("");

给新生成的元素绑定单击事件

$("#zuowei").on('click','.jiejin',function(){ });

循环插入元素

$.each(users,function(i,n){ })
