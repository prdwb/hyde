---
layout: post
title: 利用 SAE 实现新成绩提醒
---
现在到了很多大学期末的出分阶段，但是不少学校的手机助手都没有新成绩提醒的功能，那现在我就给大家演示如何利用 SAE 实现这一功能。它涉及解析 JSON 数据、SAE 上 Mail ,  Cron , 和 Storage 服务的使用等。

主要的想法是将代码部署在 SAE 上，利用 Cron 服务定时查询成绩，解析返回的 JSON 数据，将课程名和成绩取出来，存入新的数组中并计数，当下次查到的成绩个数有变化时，就调用 Mail 服务给自己发送邮件。

1. 先通过抓包等方式获得查询成绩的接口，比如我们学校的接口是这样的：

```
http://yimutest.sinaapp.com/dutserver/index.php?r=edu/scoreall&pass=password&stuid=studentID&
```

其中 password 换成密码，studentID 换成学号。

2. 在 SAE 上新建一个项目，叫做 dutscore, 再在 Storage 中新建一个 domain 叫做 dutscore，然后在其中新建一个文件，叫做 counter\_old\_mem.php，用于存放上次查询到的成绩个数，这个文件要先写入如下内容：

```php
<?php return $counter_old = 0; ?>
```

3.在代码管理中创建一个新版本的代码，在 index.php 中写入如下代码：

```php
<?php
$api = 'http://yimutest.sinaapp.com/dutserver/index.php?r=edu/scorethis&pass=password&stuid=studentID';

//初始化计数器
$counter = 0;

//向学校服务器请求成绩信息，存入变量 info
$info = file_get_contents($api);

//解析返回的 JSON 数据
$de_json = json_decode($info, true);
$list =  $de_json['result']['Score.list'];
$count = count($list);
$output = array($count);

//将课程名称和成绩取出来，存入数组 output，并统计成绩个数，存入变量 counter
for( $i = 0; $i < $count; $i ++ )
{   $output[$list[$i]['name']] = $list[$i]['score'];
	if($list[$i]['score'] != NULL)
        $counter++;
}

//从外部文件读入上次查询的成绩个数
$counter_old = include 'saestor://dutscore/counter_old_mem.php';

//如果这次查到的成绩个数与上次不同，就发送邮件通知我们
if($counter != $counter_old)
{   $to = '用于接收邮件的邮箱';
    $subject = '新成绩！';
    $msgbody = print_r($output,true);
    $smtp_user = '用于发邮件的邮箱';
    $smtp_pass = '用于发邮件的邮箱的密码';

    $mail = new SaeMail();

    $ret = $mail->quickSend($to, $subject, $msgbody, $smtp_user, $smtp_pass);

    //发送失败时输出错误码和错误信息
    if ($ret === false) {
        var_dump($mail->errno(), $mail->errmsg());
    }

    $mail->clean(); //重用此对象

    //将这次查到的成绩个数写入外部文件
    $context = '<?php return $counter_old = '.$counter.'; ?>';

    file_put_contents('saestor://dutscore/counter_old_mem.php', $context);
}

?>
```

**以上代码适用于大连理工大学，其他学校需要根据具体情况进行微调。**需要调整的地方一定有 api，可能有解析 JSON 数据的部分。

4. 接下来就要实现定时执行上一步创建的脚本，利用的是 SAE 的 Cron 服务。

在代码根目录下已有的 config.yaml 中写入如下代码：

name: dutscore<br>
version: 1<br>
accesskey: ACCESSKEY<br>
cron:<br>
&nbsp;&nbsp;&nbsp;&nbsp;- description: dutscore<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;url: index.php<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schedule: every 1 hour, from 6:00 to 23:00<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;timezone: Beijing<br>


其中 accesskey 部分填写该项目的 accesss key，url 部分填写要执行的脚本， schedule 部分填写执行的时间安排。

至此，你就完成了了一个简单的 新成绩通知 的小程序啦！

由于我是第一次接触 PHP，代码中肯定有不规范、不简洁的地方，希望大家可以指出。