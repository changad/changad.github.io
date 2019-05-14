---
layout: post
title:  "PHPMailer发送邮件"
date:   2019-05-14 16:11:13 +0000
categories: xiaochang update
---

php发送邮件之前用的php5.6就可以直接用实例化 \PHPMailer内置类   这次用的php7.2发现内置类不能用了，需要外部引入。

首先先下载PHPMailer的类库
下载地址

      https://github.com/PHPMailer/PHPMailer/
      

引入

      use PHPMailer\PHPMailer\PHPMailer;
      use PHPMailer\PHPMailer\Exception;
      
      require_once './email/Exception.php';
      require_once './email/PHPMailer.php';
      require_once './email/SMTP.php';
      
      
      function send()
      {
          require_once './email/Exception.php';
            require_once './email/PHPMailer.php';
            require_once './email/SMTP.php';

          //******************** 配置信息 ********************************
          $mail = new PHPMailer(true); 
          $mail->CharSet = 'UTF-8';           //设定邮件编码，默认ISO-8859-1，如果发中文此项必须设置，否则乱码
          $mail->IsSMTP();                    // 设定使用SMTP服务
          $mail->SMTPDebug = 0;               // SMTP调试功能 0=关闭 1 = 错误和消息 2 = 消息
          $mail->SMTPAuth = true;             // 启用 SMTP 验证功能
          $mail->SMTPSecure = 'ssl';          // 使用安全协议
          $mail->Host = \Yaconf::get('email.smtpserver'); // SMTP 服务器
          $mail->Port = 465;                  // SMTP服务器的端口号
          $mail->Username = \Yaconf::get('email.smtpusermail');    // SMTP服务器用户名
          $mail->Password = \Yaconf::get('email.smtppass');     // SMTP服务器密码
          $mail->SetFrom(\Yaconf::get('email.smtpusermail'), '河南物联网标识管理公共服务平台');
          $replyEmail = \Yaconf::get('email.smtpusermail');                   //留空则为发件人EMAIL
          $replyName = \Yaconf::get('email.smtpusermail');                    //回复名称（留空则为发件人名称）
          $mail->AddReplyTo($replyEmail, $replyName);
          $mail->Subject = \Yaconf::get('email.mailtitle'); //邮件主题
          $mail->MsgHTML(\Yaconf::get('email.mailcontent')); //内容
          $mail->AddAddress(\Yaconf::get('email.smtpemailto'), \Yaconf::get('email.mailtitle'));
          if (is_array($attachment)) { // 添加附件
              foreach ($attachment as $file) {
                  is_file($file) && $mail->AddAttachment($file);
              }
          }
          return $mail->Send() ? true : $mail->ErrorInfo;
	    
    }

