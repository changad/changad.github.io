---
layout: post
title:  "tp3使用memcache缓存技术"
date:   2017-02-06 14:47:13 +0000
categories: xiaochang update
---


1、首先配置memcache,可参考memcache缓存技术
2、配置tp框架的配置文件

          在  ThinkPHP/conf/convention.php 中 修改如下
          'DATA_CACHE_TYPE'       => 'Memcache',  // 数据缓存类型,
3、使用memcache
使用memcache缓存

          S($key,$value);
          $cache = S($key);
