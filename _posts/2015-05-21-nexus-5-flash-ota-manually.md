---
title: Nexus 5 刷入 OTA 包
layout: post
---
Nexus 5 终于收到 Android 5.1.1 的更新啦！相信很多朋友都掌握了刷入工厂镜像来升级的技巧（如果你还不熟悉，可以移步谷歌的<a href="https://developers.google.com/android/nexus/images" target="_blank">官方指南</a>）。可是，如果你觉得刷入完整镜像过于麻烦，又不想等待 OTA，那么可以参考本文介绍的方法，手动刷入 OTA 包进行升级。

注意：需要1.0.32或以上版本的 adb（文章末尾有下载）。

打开命令行，将手机重启到 Recovery 模式：

```
adb reboot recovery
```

看到小机器人倒地的图标，这时按音量上键+电源键，进到 Recovery 的菜单，选择“apply update from adb”。

下载好 OTA 包（文章末尾有下载），重命名为 update.zip ,输入：

```
adb sideload update.zip
```

等待进度走到100%就升级完毕啦！

注意：该下载包中的 OTA 包适用于从 LMY47D （5.1.0）升级到 LMY48B（5.1.1）。

**下载**：<a href="http://pan.baidu.com/s/1o6zIbME" target="_blank">百度云盘</a>(25MB)