---
title: 【翻译】破解微软Office文件密码
layout: post
---
本文翻译自<a href="http://pentestcorner.com/" target="_blank">PenTest Corner</a>的<a href="http://pentestcorner.com/cracking-microsoft-office-97-03-2007-2010-2013-password-hashes-with-hashcat/" target="_blank">Cracking Microsoft Office (97-03, 2007, 2010, 2013) password hashes with Hashcat</a>。本文讲解了如何利用Hashcat破解微软Office文件（97-03,2007,2010,2013）的密码。

<a href="http://hashcat.net/oclhashcat/" target="_blank">Hashcat</a>是一个很著名的密码破解工具，以速度快而著称。（本文以适用于 NVIDIA显卡的 cudaHashcat 为例。）

首先，你需要有一个有密码保护的 Office 文件，在这里，我使用一个 Word 2007 文档，密码为&#8221;password12345&#8243;。

第一步是从文档中提取 hash ，需要用到的工具是 <a title="office2john.py" href="https://github.com/kholia/RC4-40-brute-office/blob/master/office2john.py" target="_blank">office2john.py</a>（也可以用*john the ripper*）。

运行office2john.py:

```
office2john.py <Office_Document>
```

我们的示例结果是这样的：

```
office2john.py example.docx
example.docx:$office2007*20*128*16*3125bda60f5672f05419ae6857e11078*1f949bd0c6d642b64e1734e4bd6a0ef8*e2cbd5f857e501512a0bc9614b09762cfb312fe4
```

我们可以看到在 hash 的开头能看到 Office 版本的标识符（$office$\*2007\*）。既然得到了 hash，现在我们就要开始破解了！

你有两个选择：

1.将文件名从 hash 中删掉（一直删到“ : ”）。  
2.加入 -- username 参数 ，这样文件名就会被当成 username。

我倾向于第二个选择，因为这样我们就不用去改 hash 了。

现在，我们需要调用 cudaHashcat，格式如下：

```
cudaHashcat64.exe -a 0 -m <office_Flag> --username --status -o <Output_File> <Hash> <Dictionary>
```

参数含义如下：

-a 0：字典破解  
-m <Office_Flag>：对应Office版本的标识符  
--username：忽略用户名标识  
--status：破解过程中提示进度  
-o <Output_file>：结果存储位置  
<Hash>：提取的hash  
<Dictionary>：破解所需的字典文件。如果你没有的话，可以在<a href="http://hashcat.net/forum/thread-1236.html" target="_blank">这里</a>找到。

以下是示例命令：

```
cudaHashcat64.exe -a 0 -m 9400 --username -o found.txt  hash.txt  pass.txt
```

结果如下：

```
$office$*2007*20*128*16*3125bda60f5672f05419ae6857e11078*1f949bd0c6d642b64e1734e4bd6a0ef8*e2cbd5f857e501512a0bc9614b09762cfb312fe4:password12345
Session.Name...: cudaHashcat
Status.........: Cracked
Input.Mode.....: File (pass.txt)
Hash.Target....: $office$*2007*20*128*16*3125bda60f5672f05419ae6857e11078*1f949bd0c6d642b64e1734e4bd6a0ef8*e2cbd5f857e501512a0bc9614b09762cfb312fe4
Hash.Type......: Office 2007
Time.Started...: 0 secs
Speed.GPU.#1...: 0 H/s
Recovered......: 1/1 (100.00%) Digests, 1/1 (100.00%) Salts
Progress.......: 1/1 (100.00%)
Skipped........: 0/1 (0.00%)
Rejected.......: 0/1 (0.00%)
HWMon.GPU.#1...: 0% Util, 36c Temp, N/A Fan
```

根据你电脑显卡的不同，破解时间会有差别。如果成功破解，你会在你指定的输出文件中找到密码，它的格式是Hash:Password。