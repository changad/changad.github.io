---
layout: post
title:  "MySQL主从复制(基于GTID复制)"
date:   2018-12-08 10:11:13 +0000
categories: xiaochang create
---

基于GTID复制的优势

DTID即全局事务ID，其保证为每一个在主服务器上提交的事务在复制集群中可以生成一个唯一的ID

   GTID = suorce_id:trebcaction_id



步骤
1、 在主DB服务器上建立复制帐号

     create user '帐号'@'ip段' identified by '密码'
     grant replication alave on*.*to '帐号'@'ip段'


2、配置主库服务器

      bin_log = /usr/local/mysql/log/mysql-bin
      server_id = 100     /*唯一的*/
      gtid_model = on   
      enforce_gtid_consistency = on   /*强制gtid一致性  */ 
      log-slave-updates = on  /*mysql5.7以下版本需要配置*/

3、配置从数据库服务器

      server_id = 101
      relay_log = usr/local/mysql/log/relay_log
      gtid_mode = on
      enforce_gtid_consistency = on

      log-slave-updates = on  /*mysql5.7以下需要启动*/
      read_only = on  /*从数据库的安全性   建议*/
      master_info_repository = TABLE    /*建议*/
      relay_log_info_repository = TABLE  /*建议*/



4、初始化从服务器数据

    mysqldump --master-data =2 -single-transaction

    xtarbackup --slave-info

启动基于GTID的复制
      change master to master_host = '主ip'
                 master_user = '用户名'
                 master_password = '密码'
                 master_auto_position = 1  /*告诉从是gtid复制方式*/



优点：
可以很方便的进行故障转移
从库不会丢失主库上的任务修改

缺点：
故障处理比较复杂
