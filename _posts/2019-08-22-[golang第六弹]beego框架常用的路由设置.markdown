---
layout: post
title:  "[golang第六弹]beego框架常用的路由设置"
date:   2019-08-22 16:05:20 +0000
categories: xiaochang create
---

自己感觉在工作中比较常用的几种路由方式，总结为restful controller路由、自动匹配路由、注解路由


restful controller 路由

		  beego.Router("/api/list",&controllers.DemoController{},"get:GetName")

对应的为controller文件下的DemoController控制器的GetName方法  get请求


	DemoController实例代码

		package controllers

		import (
			"github.com/astaxie/beego"
			// "fmt"
		)

		type DemoController struct{
			beego.Controller
		}


		func (this *DemoController) GetName(){
			this.Ctx.WriteString("This is restful api")
		}

	以下是不同的 method 对应不同的函数，通过 ; 进行分割的示例：

		beego.Router("/api/list",&controllers.DemoController{},"get:GetName;post:PostName")




自动匹配路由
	控制器名/方法名

	beego.AutoRouter(&controllers.DemoController{})

	/demo/getname  调用的是DemoController中的getname方法   

	/:controller/:method的匹配之外,其他都将化为参数  用this.Ctx.Input.Params()获取

	注意：这里的路由中的控制器名和方法名都是小写


注释路由
	
	beego.Include(&controllers.DemoController{})
	无需在router中注册路由，只需要Include相应地controller，然后在controller的method方法上面写上router注释（// @router）就可以了

		//@router /liast/:id  [get]


	DemoController实例代码

		package controllers

		import (
			"github.com/astaxie/beego"
		)

		type DemoController struct{
			beego.Controller
		}

		func (c *DemoController) URLMapping(){
			c.Mapping("GetName",c.GetName)
			c.Mapping("GetInfo",c.GetInfo)

		}


		//localhost:8080/getname/1
		//@router /getname/:id  [get]
		func (this *DemoController) GetName(){
			id := this.Ctx.Input.Param(":id")
			this.Ctx.WriteString("This is restful api get请求"+id)
		}


		//localhost:8080/getinfo/1
		//@router /getinfo  [post]
		func (this *DemoController) GetInfo(){
			this.Ctx.WriteString("This is restful api post请求")
		}


	beego自动会进行源码分析，注意只会在dev模式下进行生成，生成的路由放在“/routers/commentsRouter.go”文件中。
