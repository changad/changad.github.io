---
layout: post
title:  "[golang第十一弹]redis"
date:   2019-08-24 17:07:33 +0000
categories: xiaochang create
---

首先先安装redis包
  
    go  get  github.com/astaxie/goredis
    
引用redis包

   import "github.com/astaxie/goredis"
   
   
使用实例


        func main(){
            //声明变量
            var client  goredis.Client
            //定义链接
            client.Addr = "127.0.0.1:6379"
            //set操作
            err := client.Set("test",[]byte("hellow redis"))
            checkError(err)
            //get操作
            res,err := client.Get("test")
            checkError(err)
            fmt.Println(string(res))
        }



      func checkError(err error){
          if err != nil{
            panic(err)
          }
      }
      
  
  
更多操作请查看 github源码  https://github.com/astaxie/goredis/blob/master/redis.go
  

