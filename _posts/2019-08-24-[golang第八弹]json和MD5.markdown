---
layout: post
title:  "[golang第八弹]json和MD5"
date:   2019-08-24 09:07:33 +0000
categories: xiaochang create
---

json

  内置encoding/json标准库   但是量大的时候性能比较低
  github.com/pquerna/ffjson/ffjson   性能高   量大可优选  需要安装
  两者用法一致


json编码 
  
      func Marshal(v interface{})([]type,error)

解码

      func  Unmarshal(data []type,v interface{}) error


例子

            package main

            import (
              "fmt"
              "encoding/json"
            )

            type Person struct{
              Name string `json:"person_name"`  //在json形式中name的别名person_name
              Age int
            }

            func main(){
              //对数组类型
              x := [5]int{1,2,3,4,5}
              s, err := json.Marshal(x)
              if err != nil {
                panic(err)
              }
              fmt.Println(string(s))

              //[1,2,3,4,5]


              //对map类型
              m := make(map[string]string)
              m["name"] = "zhangsan"
              s2, err2 := json.Marshal(m)
              if err2 != nil {
                panic(err2)
              }
              fmt.Println(string(s2))

              //{"name":"zhangsan"}


              //对 对象类型
              person := Person{"zhangsan",25}
              s3, err3 := json.Marshal(person) 
              if err3 != nil{
                panic(err3)
              }
              fmt.Println(string(s3))

              //{"person_name":"zhangsan","Age":25}
              //

              
              //解码
              var s4 interface{}

              json.Unmarshal([]byte(s3), &s4)

              fmt.Printf("%v",s4)

              //map[Age:25 person_name:zhangsan]

            }




MD5

内置 crypto/md5标准库

      Md5Inst := md5.New()
      Md5Inst.Write([]byte("test md5"))
      Result := Md5Inst.Sum([]byte(" "))
      fmt.Printf("%x\n\n", string(Result))
