---
layout: post
title:  "Elasticsearch分布式部署配置"
date:   2019-05-09 14:05:13 +0000
categories: xiaochang create
---


elasticsearch是天然支持分布式的

elasticsearch配置文件：elasticsearch.yml

假如有若干个节点组成了elasticsearch集群


节点1配置

    cluster.name: es     #代表集群的名字  名字相同的会自动成为一个集群
    node.name: es_1      #节点1名字
    node.master: true    #设置为master主服务


节点2配置

    cluster.name: es     #代表集群的名字  
    node.name: es_2      #节点2名字
    node.master: false

    discovery.zen.ping.unicast.hosts: ["127.0.0.1"]  #主动去寻找主服务  IP为主服务的真实IP

节点3配置

    cluster.name: es     #代表集群的名字  
    node.name: es_3      #节点3名字
    node.master: false

    discovery.zen.ping.unicast.hosts: ["127.0.0.1"]  #主动去寻找主服务  IP为主服务的真实IP
    
更多节点配置同上
