---
layout: post
title:  "[golang第十二弹]httplib使用"
date:   2019-08-24 19:07:33 +0000
categories: xiaochang create
---

httplib就相当于php curl请求

安装

   	go get  github.com/astaxie/beego/httplib
   
get请求

  	req := httplib.Get("http://www.xxx.com")
	str, err := req.String()
  
  
post请求
  
 方法1

      req := httplib.Post("http://xxx.me/")
      req.Param("username","astaxie")
      req.Param("password","123456")
  
  
  方法2
  
      req := httplib.Post("http://www.xxx.com")
      req.JSONBody(map[string]interface{}{"username":"astaxie","password":"123456"})
      var res interface{}
      req.ToJSON(&res)   //转json
      fmt.Println(res)
      
  上传大量数据
  
      req := httplib.Post("http://www.xxx.com")
      bt,err:=ioutil.ReadFile("hello.txt")
      if err!=nil{
          log.Fatal("read file err:",err)
      }
      req.Body(bt)
      
 上传文件
      
      b:=httplib.Post("http://beego.me/")
      b.Param("username","astaxie")
      b.PostFile("uploadfile1", "httplib.pdf")
      str, err := b.String()
      if err != nil {
          t.Fatal(err)
      }
      
 设置header信息
 
      req.Header("Accept-Encoding","gzip,deflate,sdch")
      req.Header("Host","beego.me")
      
支持超时

      req.SetTimeout(connectTimeout, readWriteTimeout)
      httplib.Get("http://beego.me/").SetTimeout(100 * time.Second, 30 * time.Second)


