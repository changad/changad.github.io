---
layout: post
title:  "[golang第五弹]beego框架mysql数据库操作"
date:   2019-08-22 14:05:20 +0000
categories: xiaochang create
---


1、首先引入包
	
      import （
        "github.com/astaxie/beego/orm"  
        _ "github.com/go-sql-driver/mysql"
      ）

	mysql包默认是没有的需要下载

	    go get github.com/go-sql-driver/mysql

2、	使用beego的orm链接数据库
   	beego必须注册一个别名为default的数据库，作为默认使用。

   		  orm.RegisterDataBase("default", "mysql", "test:123456@/test?charset=utf8",30,30)

   参数1：是数据库的别名，用来切换数据库使用。
   
   参数2：数据库类型
   
   参数3：数据库连接字符串:test:123456@test?charset=utf8相对于用户名:密码@数据库地址+名称?字符集
   
   参数4：数据库的最大空闲连接   相当于:orm.SetMaxIdleConns("default", 30)
   
   参数5：数据库的最大数据库连接   相当于: orm.SetMaxOpenConns("default", 30)
   
   参数4,参数5非必填，会使用数据库默认值：
  
  例子
  
         orm.RegisterDataBase("default","mysql","root:@tcp(127.0.0.1:3306)/imooc?charset=utf8",30,30)
        
	
3、 注册数据库 
    在入口文件main.go中添加init函数
    
    	 func init(){
	 	orm.RegisterDriver("mysql",orm.DRMySQL)
		orm.RegisterDataBase("default","mysql","root:@tcp(127.0.0.1:3306)/imooc?charset=utf8",30,30)
	 }
          
      
4、 可以使用bee generate命令自动生成curd数据库操作代码
      
        bee generate scaffold user -fields="id:int64,name:string,gender:int,age:int" -driver=mysql -conn="root:@tcp(127.0.0.1:3306)/imooc
