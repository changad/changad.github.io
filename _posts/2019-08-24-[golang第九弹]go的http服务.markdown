---
layout: post
title:  "[golang第九弹]go的http服务"
date:   2019-08-24 12:07:33 +0000
categories: xiaochang create
---

使用net/http包的 http.ListenAndServe（）方法 监听指定地址 开启http服务
参数1：监听地址
参数2:服务器处理程序 通常为空

go的http服务的简单搭建实例

        package main 
        
        import (
          "fmt"
          "net/http"
        )
        
        func main(){
            http.HandleFunc("/", IndexHandler)
            http.ListenAndServe("127.0.0.0:8080", nil)
        }
        
        func IndexHandler(w http.ResponseWriter, r *http.Request) {
            fmt.Fprintln(w, "hello world")

        }
        

        
get/post请求

需要注意的是 请求的url必须要有http://

使用之前需要引入一下两个包 

      "net/http"
      "io/ioutil" 
      
get请求

        resp, err:= http.Get("http://www.xxx.com/")
        if err != nil{
          panic（err）
        }
        defer resp.Body.Close()
        body, errr := ioutil.ReadAll(resp.Body)
        if errr != nil{
            panic（err）
        }
        fmt.Println(string(body))
        
        
post请求
又分为http.post、http.postForm、http.Do请求

  http.post请求
  
        resp, err := http.Post("http://www.xxx.com/",
        "application/x-www-form-urlencoded",
        strings.NewReader("name=golang"))
        if err != nil {
             // error
        }
        defer resp.Body.Close()
        body, err := ioutil.ReadAll(resp.Body)
        if err != nil {
            // error
        }
        fmt.Println(string(body))
        
         
http.postForm请求    需要引入 "net/url"
    
        data := make(url.Values) 
        data[“name”] = []string{“golang”} 
        
        resp, err := http.PostForm("http://www.xxx.com/", data)
        defer resp.Body.Close()
        body, err := ioutil.ReadAll(resp.Body)
        if err != nil {
            // error
        }
        fmt.Println(string(body))
        
      
http.do请求  
      
        client := &http.Client{}
 
        req, err := http.NewRequest("POST", "http://www.xxx.com/", strings.NewReader("name=golang"))
        if err != nil {
            //  error
        }
        
        req.Header.Set("Content-Type", "application/x-www-form-urlencoded")

        resp, err := client.Do(req)

        defer resp.Body.Close()

        body, err := ioutil.ReadAll(resp.Body)
        if err != nil {
            //  error
        }

