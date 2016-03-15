---
title: SAE绑定独立域名并设置域名解析
layout: post
---
在上一篇博文中，给大家介绍了如何在SAE上搭建WordPress，相信很多人都会希望自己的个人博客拥有独立的域名，下面我就介绍一下如何在SAE上绑定独立域名并完成域名解析。

首先，你要在域名注册商处注册好一个域名，具体过程不在此赘述。登陆SAE，进入应用首页，在独立域名处点击“绑定”。

[<img class="alignnone size-full wp-image-19" src="http://prdwb.github.io/images/2015/05/Image-001.png" alt="Image 001" width="269" height="123" />][1]

输入域名，点击“绑定”。

&nbsp;

[<img class="alignnone size-medium wp-image-20" src="http://prdwb.github.io/images/2015/05/Image-003-300x269.png" alt="Image 003" width="300" height="269" />][2]

至此，独立域名绑定成功，下面我们需要完成域名解析，这样访问域名时才能转到我们的网站。

在独立域名处点击“域名管理”。

[<img class="alignnone size-medium wp-image-21" src="http://prdwb.github.io/images/2015/05/Image-004-300x84.png" alt="Image 004" width="300" height="84" />][3]

查看域名解析的要求。

[<img class="alignnone size-full wp-image-23" src="http://prdwb.github.io/images/2015/05/Image-0051.png" alt="Image 005" width="847" height="269" />][4]

按要求操作，每个人的具体要求都会不同。

以图中为例，我需要把e83dfb0622.qingcheng.info通过A记录解析到10.215.147.12已完成域名验证，并将域名CNAME到相应地址以完成域名解析。

在此解释几个名词：

**A记录**：A (Address) 记录是用来指定主机名（或域名）对应的IP地址记录。

**CNAME**：CNAME是一个别名记录( Canonical Name )。当 DNS 系统在查询CNAME左面的名称的时候，都会转向CNAME右面的名称再进行查询，一直追踪到最后的PTR或A名称，成功查询后才会做出回应。与A记录不同的是，CNAME别名记录设置的可以是一个域名的描述而不一定是IP地址。

具体操作如下：

首先，登陆该域名的域名注册商，在此以GoDaddy为例。如果你的域名是在其他注册商注册的，或者你要使用另外的解析服务，操作也是类似的。

进入域名的解析设置，先添加一条A记录（下图中的第二条），以完成要求中的域名验证（你需要把参数替换成自己的）。

[<img class="alignnone size-full wp-image-24" src="http://prdwb.github.io/images/2015/05/Image-006.png" alt="Image 006" width="901" height="178" />][5]

然后，添加一条CNAME记录（下图中第二条）。

[<img class="alignnone size-full wp-image-25" src="http://prdwb.github.io/images/2015/05/Image-007.png" alt="Image 007" width="895" height="188" />][6]

值得一提的是，如果你想实现访问裸域也能转到网站（即访问http://example.com而不是http://www.example.com）的效果，你就需要再添加一条A记录（A记录图片中的一条），对应的ip地址是Ping CNAME地址得到的。

由于DNS缓存的存在，可能需要稍等一会域名解析才能生效。

至此，你就拥有了带独立域名的个人博客！

&nbsp;

 [1]: http://prdwb.github.io/images/2015/05/Image-001.png
 [2]: http://prdwb.github.io/images/2015/05/Image-003.png
 [3]: http://prdwb.github.io/images/2015/05/Image-004.png
 [4]: http://prdwb.github.io/images/2015/05/Image-0051.png
 [5]: http://prdwb.github.io/images/2015/05/Image-006.png
 [6]: http://prdwb.github.io/images/2015/05/Image-007.png