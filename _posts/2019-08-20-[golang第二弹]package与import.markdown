---
layout: post
title:  "[golang第二弹]package与import"
date:   2019-08-20 19:07:13 +0000
categories: xiaochang create
---

go 一个完整的可执行程序,需要package声明包，import引入包  main()主函数

  package main 
  import "fmt"
  
  func main(){
       
  }
 

go语言里的package类似与php中的命名空间，且必须在开头第一行

同一路径下只能存在一个package

比如 在show目录下 有个show.go文件
则定义 package show   

在mian.go文件中用  
import‘show’  来引用show.go文件

注意：l类似show.go这些自定义包里的所有函数的函数名必须大写首字母，不然外部包找不到


mport特殊用法

1）别名用法

    import (
       aaa  'bbb'	
    )


2）下划线（_）

     import(
       _ 'bbb
    )
导入的包只执行包中的init函数，其他函数无法被调用
