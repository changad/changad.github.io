---
layout: post
title:  "swoole的task异步任务处理机制"
date:   2019-08-19 17:05:13 +0000
categories: xiaochang update
---


首先先创建一个tcp服务

        $this->ws=new \swoole_websocket_server("0.0.0.0",9501);
        $this->ws->set([
            //启动task必须要设置其数量
            'worker_num' => 4,
            'task_worker_num' => 2,
        ]);
        //监听新端口
        $this->server=$this->ws->listen("127.0.0.1", 9502, SWOOLE_SOCK_TCP);
        //关闭websocket模式
        $this->server->set([
            'open_websocket_protocol' => false,
        ]);
        
        //在监听数据接收事件时执行task任务
        public function onReceive($serv, $fd, $from_id, $data)
        {
            //执行异步任务
            $this->ws->task($data);

        }
        
用tcp链接传输数据

      $cli = new \swoole_client(SWOOLE_TCP);
      //创建链接
      $cli->connect('127.0.0.1', 9502); 
      //发送数据
      $cli->send($this->msg);
        
