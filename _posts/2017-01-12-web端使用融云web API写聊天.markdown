---
layout: post
title:  "web端使用融云web API写聊天"
date:   2017-01-12 14:47:13 +0000
categories: xiaochang update
---



1、 首先引入融云官方js

        <script src="http://cdn.ronghub.com/RongIMLib-2.2.4.min.js">


2、 获取用户token，这里获取token请参考之前写的tp5整合融云开发聊天功能

3、连接融云服务器


                //初始化SDK
                RongIMLib.RongIMClient.init("xxxxxxx"); //这里的值是App Key

                var chatRoomId = "10001"; // 聊天室 Id。
                var count = 10;// 拉取最近聊天最多 50 条。
                var counts = 30; // 获取聊天室人数 （范围 0-20 ）
                var order = RongIMLib.GetChatRoomType.REVERSE;// 排序方式。
                var token = "BI+NeCDsvTBpNXzDJiJpoE5ZRRA1upo9ZcpS6go5h8UQ9M4Q+ISpbHXidCGbFvmapYgP6d14L1l/ux3ULVLCnw==";
                 // 连接融云服务器。
                  RongIMClient.connect(token, {
                    onSuccess: function(userId) {
                      console.log("Login successfully." + userId);
                    },
                    onTokenIncorrect: function() {
                      console.log('token无效');
                    },
                    onError:function(errorCode){
                          var info = '';
                          switch (errorCode) {
                            case RongIMLib.ErrorCode.TIMEOUT:
                              info = '超时';
                              break;
                            case RongIMLib.ErrorCode.UNKNOWN_ERROR:
                              info = '未知错误';
                              break;
                            case RongIMLib.ErrorCode.UNACCEPTABLE_PaROTOCOL_VERSION:
                              info = '不可接受的协议版本';
                              break;
                            case RongIMLib.ErrorCode.IDENTIFIER_REJECTED:
                              info = 'appkey不正确';
                              break;
                            case RongIMLib.ErrorCode.SERVER_UNAVAILABLE:
                              info = '服务器不可用';
                              break;
                          }
                          console.log(errorCode);
                        }
                  });
                 // 设置连接监听状态 （ status 标识当前连接状态）
                 // 连接状态监听器
                 RongIMClient.setConnectionStatusListener({
                    onChanged: function (status) {
                        switch (status) {
                            //链接成功
                            case RongIMLib.ConnectionStatus.CONNECTED:
                                console.log('链接成功');
                                //当连接服务器成功时加入聊天室
                                RongIMClient.getInstance().joinChatRoom(chatRoomId, count, {
                                      onSuccess: function() {
                                           alert("加入聊天室成功");
                                //获取聊天室信息
                               RongIMClient.getInstance().getChatRoomInfo(chatRoomId, count, order, {
                                     onSuccess: function(chatRoom) {
                                         //console.log(chatRoom);
                                      console.log(chatRoom.userInfos); 
                                       //console.log(chatRoom.userTotalNums);
                                        // chatRoom => 聊天室信息。
                                       // chatRoom.userInfos => 返回聊天室成员。
                                       // chatRoom.userTotalNums => 当前聊天室总人数。
                              },
                              onError: function(error) {
                                  // 获取聊天室信息失败。
                                  alert("获取聊天室信息失败");
                                  } 
                             });
                                      },
                              onError: function(error) {
                                  alert("加入聊天室失败");
                                // 加入聊天室失败
                                }
                               });
                                break;
                            //正在链接
                            case RongIMLib.ConnectionStatus.CONNECTING:
                                console.log('正在链接');
                                break;
                            //重新链接
                            case RongIMLib.ConnectionStatus.DISCONNECTED:
                                console.log('断开连接');
                                break;
                            //其他设备登录
                            case RongIMLib.ConnectionStatus.KICKED_OFFLINE_BY_OTHER_CLIENT:
                                console.log('其他设备登录');
                                break;
                              //网络不可用
                            case RongIMLib.ConnectionStatus.NETWORK_UNAVAILABLE:
                              console.log('网络不可用');
                              break;
                            }
                    }});
                // 消息监听器
                RongIMClient.setOnReceiveMessageListener({
                            // 接收到的消息
                            onReceived: function (message) {
                                // 判断消息类型
                                switch(message.messageType){
                                    case RongIMClient.MessageType.TextMessage:
                                           // 发送的消息内容将会被打印
                                        console.log(message);
                                    /**
                                    ** 这里接收的是其他用户发送的消息，将聊天信息DOM操作写入页面即可
                                    **/              
                                        break;
                                    case RongIMClient.MessageType.VoiceMessage:
                                        // 对声音进行预加载                
                                        // message.content.content 格式为 AMR 格式的 base64 码
                                        RongIMLib.RongIMVoice.preLoaded(message.content.content);
                                        break;
                                    case RongIMClient.MessageType.ImageMessage:
                                        // do something...
                                        break;
                                    case RongIMClient.MessageType.DiscussionNotificationMessage:
                                        // do something...
                                        break;
                                    case RongIMClient.MessageType.LocationMessage:
                                        // do something...
                                        break;
                                    case RongIMClient.MessageType.RichContentMessage:
                                        // do something...
                                        break;
                                    case RongIMClient.MessageType.DiscussionNotificationMessage:
                                        // do something...
                                        break;
                                    case RongIMClient.MessageType.InformationNotificationMessage:
                                        // do something...
                                        break;
                                    case RongIMClient.MessageType.ContactNotificationMessage:
                                        // do something...
                                        break;
                                    case RongIMClient.MessageType.ProfileNotificationMessage:
                                        // do something...
                                        break;
                                    case RongIMClient.MessageType.CommandNotificationMessage:
                                        // do something...
                                        break;
                                    case RongIMClient.MessageType.CommandMessage:
                                        // do something...
                                        break;
                                    case RongIMClient.MessageType.UnknownMessage:
                                        // do something...
                                        break;
                                    default:
                                        // 自定义消息
                                        // do something...
                                }
                            }
                        });

                $('.button').click(function(data){
                    var content = $('.textarea').val();
                //发送消息
                    var msg = new RongIMLib.TextMessage({content:content,extra:"admin"});
                        var conversationtype = RongIMLib.ConversationType.CHATROOM; // 选择相应的消息类型即可，这里是聊天室模式。
                        var targetId = "10001"; // 目标 Id
                       RongIMClient.getInstance().sendMessage(conversationtype, targetId, msg, {
                                onSuccess: function (message) {
                                    //message 为发送的消息对象并且包含服务器返回的消息唯一Id和发送消息时间戳
                                    console.log(message);
                                   /**
                                    ** 这里当前用户发送的消息，将聊天信息DOM操作写入页面即可
                                    **/

                                  //var content = message.content.content;
                                  // var username = 'admin';
                                  // var settime = message.sentTime;
                                },
                                onError: function (errorCode,message) {
                                    var info = '';
                                    switch (errorCode) {
                                        case RongIMLib.ErrorCode.TIMEOUT:
                                            info = '超时';
                                            break;
                                        case RongIMLib.ErrorCode.UNKNOWN_ERROR:
                                            info = '未知错误';
                                            break;
                                        case RongIMLib.ErrorCode.REJECTED_BY_BLACKLIST:
                                            info = '在黑名单中，无法向对方发送消息';
                                            break;
                                        case RongIMLib.ErrorCode.NOT_IN_DISCUSSION:
                                            info = '不在讨论组中';
                                            break;
                                        case RongIMLib.ErrorCode.NOT_IN_GROUP:
                                            info = '不在群组中';
                                            break;
                                        case RongIMLib.ErrorCode.NOT_IN_CHATROOM:
                                            info = '不在聊天室中';
                                            break;
                                        default :
                                            info = x;
                                            break;
                                    }
                                    alert('发送失败:' + info);
                                }
                            }
                        );

                  });

