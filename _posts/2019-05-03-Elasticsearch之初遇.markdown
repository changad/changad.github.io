---
layout: post
title:  "Elasticsearch之初遇"
date:   2019-05-03 16:05:13 +0000
categories: xiaochang create
---


elasticsearch是一个近实时的搜索平台。

知识点

集群： 一个或者多个elasticsearch服务组成

节点： 集群中的一个独立elasticsearch服务

索引： 相同特征的文档（document）的集合，可以比喻为一个数据库

类型： 同一个索引中可以存储不同类型的文档，可以比喻为一个数据表

文档： 文档是索引的基础单元，使用json,可以比喻为表中的一条数据

分片和副本： 一般选为节点的1.5到3倍最佳

