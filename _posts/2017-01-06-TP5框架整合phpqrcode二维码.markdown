---
layout: post
title:  "TP5框架整合phpqrcode二维码"
date:   2017-01-06 17:05:13 +0000
categories: xiaochang update
---



前段时间写了一个App接口的项目，用到了二维码。在网上查了查资料，找了个简单的方法，把步骤梳理一下。

项目用的是tp5的框架，首先在tp中的composer.json文件中的 require 段加上 "endroid/qrcode": "1.*@dev"，然后用git版本控制器或者cmd命令进入项目目录中，执行composer update命令，将二维码包更新到本地项目中。

 
       将控制器中添加生成二维码的方法
         public function videoQrcodes()
        {        
            //生成当前的二维码
            $qrCode = new \Endroid\QrCode\QrCode();       
            //想显示在二维码中的文字内容，这里设置了一个查看文章的地址
            $url = 'project.cn/admin/admin/videoCates?id='.$id;   //扫二维码跳转的地址
            $qrCode->setText($url)
                ->setSize(300)
                ->setPadding(10)
                ->setErrorCorrection('high')
                ->setForegroundColor(array('r' => 0, 'g' => 0, 'b' => 0, 'a' => 0))
                ->setBackgroundColor(array('r' => 255, 'g' => 255, 'b' => 255, 'a' => 0))
                ->setLabel('')
                ->setLabelFontSize(16)
                ->setImageType(\Endroid\QrCode\QrCode::IMAGE_TYPE_PNG);
             $qrCode->render();
        }
前端的html页面在二维码的位置添加

         img  src="{:url('admin/admin/videoQrcodes')}?id={$vo.id}"
url的值指定后台控制器生成二维码的方法
