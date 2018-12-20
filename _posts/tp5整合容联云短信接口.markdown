---
layout: post
title:  "tp5整合容联云短信接口"
date:   2017-01-09 14:47:13 +0000
categories: xiaochang update
---

首先先去www.yuntongxun.com容联云官网注册个账号。获取开发者的 主账号 accountSid、主账号token accountToken、应用Id appid。

在模块文件下的config.php中配置添加常量配置：

                    return [
                        'RONGLIAN_ACCOUNT_SID'   => 'xxx', //容联云通讯 主账号 accountSid
                        'RONGLIAN_ACCOUNT_TOKEN' => 'xxx', //容联云通讯 主账号token accountToken
                        'RONGLIAN_APPID'         => 'xxx', //容联云通讯 应用Id appid
                    ];
                  
最后在控制器中添加业务代码方法

                             /**
                        **短信验证
                              **@param phone 手机号
                              ** return  
                        **/
                        public function getCode()
                        {
                        $phone = input('get.phone');
                        //需要发送的验证码
                        $code = $this->random(4,1);
                              //引入容联云sdk,  这里的sdk放在vendor里的note文件中
                        vendor('note.CCPRestSDK');		
                              //请求地址，格式如下，不需要写https://  
                              $serverIP='app.cloopen.com';  
                              //请求端口  
                              $serverPort='8883';  
                              //REST版本号  
                              $softVersion='2013-12-26';  

                              //主帐号  
                              $accountSid=config('RONGLIAN_ACCOUNT_SID');  
                              //主帐号Token  
                              $accountToken=config('RONGLIAN_ACCOUNT_TOKEN'); 
                              //应用Id  
                              $appId=config('RONGLIAN_APPID');  
                              //实例化类
                              $rest = new \REST($serverIP,$serverPort,$softVersion);  
                              $rest->setAccount($accountSid,$accountToken);  
                              $rest->setAppId($appId);  
                              // 发送模板短信  
                              $result=$rest->sendTemplateSMS($phone,array($code,5),1);  
                               if($result == NULL ) {
                                echo 0;die;
                             }
                             if($result->statusCode!=0) {
                                echo 0;die;
                                 //TODO 添加错误处理逻辑
                             }else{
                                      //清楚之前的code
                              Cookie::set('code',null);
                                     //将手机验证码存入Cookie中
                              Cookie::set('code',$code,120);
                                 //TODO 添加成功处理逻辑
                             } 	
                        }


                    /*
                    *  获取随机数短信验证码的方法
                    */
                    private function random($length = 6 , $numeric = 0) {
                            PHP_VERSION < '4.2.0' && mt_srand((double)microtime() * 1000000);
                      if($numeric) {
                        $hash = sprintf('%0'.$length.'d', mt_rand(0, pow(10, $length) - 1));
                      } else {
                        $hash = '';
                        $chars = 'ABCDEFGHJKLMNPQRSTUVWXYZ23456789abcdefghjkmnpqrstuvwxyz';
                        $max = strlen($chars) - 1;
                        for($i = 0; $i < $length; $i++) {
                          $hash .= $chars[mt_rand(0, $max)];
                        }
                      }
                        return $hash;
                    }
最后附上容联云sdk
