# 自定义表单信息同步邮箱

## 主机环境要求

1. 主机`465`端口是开启和放行的
2. PHP扩展`openssl`是开启的
3. PHP扩展`sockets`是开启的

## 发信箱要求（网易163邮箱举例子）

1. 一个网易邮箱
2. 网易邮箱后台设置`POP3/SMTP服务`为开启状态
3. 网易邮箱后台设置`IMAP/SMTP服务`为开启状态
4. 网易邮箱后台获取`授权码`（主要是针对非网易邮箱客户端登录时候需要使用的密码）

## 织梦后台设置：系统 - 系统基本参数 - 核心设置

1. 是否启用SMTP方式发送邮件：是
2. SMTP服务器：`ssl://smtp.163.com` （如果采用加密端口的话，就使用ssl://，非加密 端口则不带ssl://）
3. SMTP服务器端口：`465`
4. SMTP服务器的用户邮箱：`***@163.com` （举例邮箱，正式使用的时候用自己的）
5. SMTP服务器的用户帐号：`***@163.com` （举例邮箱，正式使用的时候用自己的）
6. SMTP服务器的用户密码：`填你邮箱授权码，不是邮箱登录密码`

## 底层编码修改

涉及文件修改：`/plus/diy.php`

找到如下代码

```php
$id = $dsql->GetLastID();
```

紧接其后添加如下代码

```php
$mailtitle = "{$diy->name}--留言通知"; 
// $mailtitle = $name."【产品询价】{dede:global.cfg_webname/}"; 如果你追求完美，标题可以采用这个
$mailbody = '';
foreach($diy->getFieldList() as $field=>$fieldvalue)
{
	$mailbody .= "{$fieldvalue[0]}:{${$field}}\r\n";
}
// $headers = "From: ".$cfg_adminemail."Reply-To: ".$cfg_adminemail;  // 我没试过这种写法
$headers = $cfg_adminemail;
if($cfg_sendmail_bysmtp == 'Y' && !empty($cfg_smtp_server))
{
	$mailtype = 'TXT';
	$mailto = 'sales@****.com'; // 这个你可以调用后台自己加的变量
	require_once(DEDEINC.'/mail.class.php');
	$smtp = new smtp($cfg_smtp_server,$cfg_smtp_port,true,$cfg_smtp_usermail,$cfg_smtp_password);
	$smtp->debug = false;//发送不成功把false改成1，再提交看错误信息
	$smtp->sendmail($mailto,$cfg_webname ,$cfg_smtp_usermail, $mailtitle, $mailbody, $mailtype);
}
else
{
	@mail($mailto, $mailtitle, $mailbody, $headers);
}
```

完成，结束，鸣谢[Dedehtml.com](https://www.dedehtml.com/notes/diyform-email.html)

[diy.zip 下载][1]


  [1]: http://www.molerose.com/usr/uploads/2019/06/3814491816.zip