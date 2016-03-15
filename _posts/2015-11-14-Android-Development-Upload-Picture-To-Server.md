---
layout: post
title: 安卓开发：上传图片到服务器
---
### 本篇博文总结自[该视频](https://www.youtube.com/watch?v=TMnQJKtmOd4)。

## 概览
将图片上传至服务器分为两个部分：安卓客户端和服务器端。本文的演示将以上传到SAE为例。

## 服务器端
上传图片采用 `http post`，需要先将照片进行 `base64` 编码，所以在服务器上需先进行解码，再存储。

### PHP源码

```php
<?php
	// 获取提交的参数
	$name = $_POST["name"];
	$id = $_POST["id"];
	$image = $_POST["image"];

	// base64解码
	$decodedImage = base64_decode($image);

	// 存入SAE的Storage，其中"picture"是建好的domain，pic.jpg是domain中已经有的照片
	// 也就是说，每次存储都会更改同一张照片
	file_put_contents('saestor://picture/pic.jpg', $decodedImage);

	// 如果要还给客户端返回信息，可以写在下面
?>
```

## 安卓客户端
现将图片转为 `Bitmap`，进行 `base64` 编码后再使用 `AsyncTask` 上传至服务器。

### 注意
这里使用到的 `Apache HTTP Client` 在 Android 6.0 中已被去除，如果仍要使用的话，需在 `build.gradle` 中做如下声明：

```
android {
    useLibrary 'org.apache.http.legacy'
}
```
具体可以看[官方的说明](http://developer.android.com/about/versions/marshmallow/android-6.0-changes.html#behavior-apache-http-client)。

### Java源码
[MainActivity.java](https://gist.github.com/prdwb/336f165ba5af4bb4fe1b)
