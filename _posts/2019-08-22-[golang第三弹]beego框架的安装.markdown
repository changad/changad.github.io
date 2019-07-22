---
layout: post
title:  "[golang第三弹]beego框架的安装"
date:   2019-08-22 10:05:20 +0000
categories: xiaochang create
---

1、安装go  

2、配置GOPATH、GOROOT

3、安装Git 
   Git 的工作需要调用 curl，zlib，openssl，expat，libiconv 等库的代码，所以需要先安装这些依赖工具。

   linux安装Git
   Ubuntu

   		$ apt-get install libcurl4-gnutls-dev libexpat1-dev gettext \  libz-dev libssl-dev$ apt-get install git$ git --versiongit version 1.8.1.2

   Centos

   		$ yum install curl-devel expat-devel gettext-devel \  openssl-devel zlib-devel$ yum -y install git-core$ git --versiongit version 1.7.1

4、安装beego
 	
 	1）进入GOPATH对应的目录 cd gowork

 	2）go get命令安装beego

 	   F:\gowork> go get github.com/astaxie/beego

 	3) 安装bee工具

 		F:\gowork> go get github.com/beego/bee 

 		cmd 输入 bee报错
 	    bee' 不是内部或外部命令，也不是可运行的程序
		或批处理文件。

		解决办法：进入GOPATH里的bin目录下，将bin目录路径添加到path环境变量

	4) 创建项目

	    进入 F:\gowork\src>  bee new webapp

	   目录结构如下

	        webapp
				|-- conf
				|   `-- app.conf
				|-- controllers
				|   `-- default.go
				|-- main.go  //入口文件
				|-- models
				|-- routers
				|   `-- router.go
				|-- static
				|   |-- css
				|   |-- img
				|   `-- js
				|-- tests
				|   `-- default_test.go
				`-- views
				    `-- index.tpl

	5）运行项目
		进入项目目录

	     F:\gowork\src\webapp>bee run

	    访问localhost:8080
