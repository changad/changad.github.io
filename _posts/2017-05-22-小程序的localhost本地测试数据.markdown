---
layout: post
title:  "小程序的localhost本地测试数据"
date:   2017-01-06 17:05:13 +0000
categories: xiaochang update
---

近些日子，小程序火的不能再火了，是个开发人员说没听说过小程序那都不好意思说自己是搞开发的，张小龙在微信的年会上说1月9号将推出小程序，小程序秉着用完即走，17年初势必会迎来一股热潮

最近也是查看各种微信小程序的文档，试了试可不可以在本地进行后端的数据交互，

通过对form标签绑定的formSubmit事件
           form catchsubmit="formSubmit"  
表单提交的数据在js中接收

     formSubmit: function(e) {
        var value =  e.detail.value;  //表单提交过来的数据
        console.log(value);
    }
通过wx.request小程序的网络请求接口向后台发送数据。注意：wx.request只支持 HTTPS 请求，且同时只能有5个网络请求连接

     wx.request({
          url: 'http：//localhost/tp5/admin',
          data: {
            phone ： value.phone,
            password : value.password,
          },
          method:'GET',
          header: {
              'Content-Type': 'application/json'
          },
          success: function(res) {
            that.get_data();   //请求成功后返回的值

          }
        })
最后在后台接受数据判断，返回即可

    public  function    index
    {
            $phone = input('get.phone');
            $password = input('get.password ');
            //判断
            echo   1；   //返回值
    }
