---
layout: post
title:  "Elasticsearch首次启动失败需要加入的配置参数"
date:   2019-05-08 12:05:13 +0000
categories: xiaochang create
---

elasticsearch配置文件：elasticsearch.yml

首次启动失败需要加入的配置参数


      xpack.ml.enabled: false
 
      network.host: 0.0.0.0    #内网外网都可以访问
      http.port: 8301          #设置端口号

      #memory 内存的配置
     
      bootstrap.memory_lock: false
      bootstrap.system_call_filter: false
      
      #http 跨域处理
      http.cors.enabled: true 
      http.cors.allow-origin: “*”
      
      
 注意每个配置项的冒号后面都要留一个空格
