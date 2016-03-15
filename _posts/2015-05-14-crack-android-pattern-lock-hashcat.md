---
title: 破解安卓图案锁屏密码
layout: post
---
安卓的图案解锁由九个点构成，按顺序为0x00-0x08（十六进制）排列如下：

00       01       02

03       04       05

06       07       08

这九个点按解锁图案的顺序排出，以十六进制的形式进行了SHA1加密，得到的密文存放于 /data/system/gesture.key

所以第一步就是将 gesture.key 导入电脑。

将手机连到电脑上，打开命令提示符，利用 adb shell 将 gesture.key 先复制到 SD 卡中（需要手机已root）：

```
adb shell
shell@android:/ $ su
su
root@android:/ # cp /data/system/gesture.key /sdcard/
cp /data/system/gesture.key /sdcard/
```

按下 Ctrl + C 退出shell，再将 gesture.key 从 SD 卡中提取到电脑里：

```
adb pull /sdcard/gesture.key D:/0514
0 KB/s (20 bytes in 1.000s)
```

将 gesture.key 用十六进制编辑器打开，得到SHA1加密的密文（如下图）。

[<img class="alignnone size-full wp-image-94" src="http://prdwb.github.io/images/2015/05/Image-041.png" alt="安卓图案解锁加密SHA1" width="746" height="92" />][1]

把得到密文 3f73e89ab651a9f6701c432230593f262f215956 复制到 hash.txt 中，方便接下来的操作。

下一步，我们就要用 Hashcat 来暴力破解这段密文。

示例命令如下：

```
cudaHashcat64.exe -m 100 -a 3 --hex-charset -i --increment-min 4 --increment-max 9 -o results.txt hash.txt ?b?b?b?b?b?b?b?b?b
```

参数解释：

-m 100 : hash 类型为SHA1  
-a 3 : 破解模式为暴力破解  
&#8211;hex-charset : 密文按十六进制处理  
-i :  掩码位数增加  
&#8211;increment-min 4 : 掩码最小长度为4，因为锁屏图案最少连4个点  
&#8211;increment-max 9 : 掩码最大长度为9，因为锁屏图案最多连9个点  
-o results.txt : 结果输出至 results.txt  
hash.txt : hash 存放的文件  
?b?b?b?b?b?b?b?b?b : 掩码，?b 代表0x00-0xff

得到的结果如下：

```
Session.Name...: cudaHashcat
Status.........: Cracked
Input.Mode.....: Mask (?b?b?b?b) [4]
Hash.Target....: 3f73e89ab651a9f6701c432230593f262f215956
Hash.Type......: SHA1
Time.Started...: Thu May 14 20:12:07 2015 (1 sec)
Speed.GPU.#1...:   110.0 MH/s
Recovered......: 1/1 (100.00%) Digests, 1/1 (100.00%) Salts
Progress.......: 125829120/4294967296 (2.93%)
Skipped........: 0/125829120 (0.00%)
Rejected.......: 0/125829120 (0.00%)
Restore.Point..: 491520/16777216 (2.93%)
HWMon.GPU.#1...: 94% Util, 42c Temp, N/A Fan
```

可以看到破解成功，那么就能在你指定的输出路径中看到密码了。由于图案复杂程度和你计算机性能的差异，破解时间会有不同。

在这个例子中，我得到的结果为

3f73e89ab651a9f6701c432230593f262f215956:$HEX[01030407]

最后的中括号里就是密码啦！

&nbsp;

 [1]: http://prdwb.github.io/images/2015/05/Image-041.png