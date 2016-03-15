---
title: 在搬瓦工搭建Shadowsocks
layout: post
---
随着网络的发展，越来越多的人希望通过互联网看到更大的世界，在这个过程中，一个好的代理不可或缺。由于免费的代理有很多限制，所以不少人都想搭建自己的代理。本文选择的方案成本极低，**每年只需25元人民币**，而且整个过程中都是图形化的操作界面，不需要任何编程基础。

Bandwagon Host是美国的一家VPS服务商，我们登陆他的官网，<a href="https://bandwagonhost.com/aff.php?aff=3303" target="_blank">Bandwagon Host 官网</a> ，点击右上角的&#8221;Register&#8221;（注册）。

[<img class="alignnone size-full wp-image-33" src="http://prdwb.github.io/images/2015/05/Image-008.png" alt="Bandwagon注册" width="126" height="50" />][1]

按要求填写，全部用拼音或英文填写。值得注意的是，国家和地区以及邮箱必须填写真实信息，因为Bandwagon Host有一个防欺诈机制，如果探测到的IP与填写的国家不符，注册就会不通过。除这几项的其他项目可以不填真实信息。填好验证码，勾选复选框（I have read and agree to the Terms of Service），点击&#8221;Register&#8221;。

[<img class="alignnone size-full wp-image-34" src="http://prdwb.github.io/images/2015/05/Image-012.png" alt="注册Bandwagon Host" width="1070" height="698" />][2]

注册完成后，进入<a href="https://bandwagonhost.com/cart.php" target="_blank">购买页面</a> ，开始购买服务器。如果仅是日常上网实用的话，推荐大家购买最便宜的版本，也就是下图中的那一款。每年3.99美元，合人民币不到25元。点击&#8221;Order Now&#8221;。

[<img class="alignnone size-full wp-image-35" src="http://prdwb.github.io/images/2015/05/Image-013.png" alt="购买Bandwagon服务器" width="979" height="175" />][3]

选择服务器地区时，推荐选择西海岸的 Arizona。点击&#8221;Add to Cart&#8221;。

[<img class="alignnone size-full wp-image-36" src="http://prdwb.github.io/images/2015/05/Image-014.png" alt="购买Bandwagon Host服务器" width="773" height="213" />][4]

接下来是确认界面，核对总计费用(Total Recurring)是每年3.99美元，点击&#8221;Checkout&#8221;。

[<img class="alignnone size-full wp-image-37" src="http://prdwb.github.io/images/2015/05/Image-015.png" alt="购买Bandwagon Host服务器" width="1042" height="187" />][5]

接下来是结算界面，只能选择PayPal付款。勾选复选框(I have read and agree to the Terms of Service)。点击&#8221;Complete Order&#8221;。

[<img class="alignnone size-full wp-image-38" src="http://prdwb.github.io/images/2015/05/Image-016.png" alt="购买Bandwagon服务器" width="950" height="199" />][6]

使用PayPal付款结束后，点击“返回到IT7 Networks Inc”。再点击&#8221;<a href="https://bandwagonhost.com/clientarea.php" target="_blank">Click here to go to your Client Area</a>&#8220;来到用户管理界面。

[<img class="alignnone size-full wp-image-39" src="http://prdwb.github.io/images/2015/05/Image-017.png" alt="Image 017" width="173" height="71" />][7][<img class="alignnone size-full wp-image-40" src="http://prdwb.github.io/images/2015/05/Image-018.png" alt="Image 018" width="274" height="40" />][8]

点击&#8221;Services&#8221;下的&#8221;My Services&#8221;。

[<img class="alignnone size-full wp-image-41" src="http://prdwb.github.io/images/2015/05/Image-019.png" alt="Bandwagon" width="213" height="169" />][9]

可以看到，刚刚购买的服务器已经开通（Status显示Active），如尚未开通，只需稍等片刻再刷新页面即可。

[<img class="alignnone size-full wp-image-42" src="http://prdwb.github.io/images/2015/05/Image-020.png" alt="Bandwagon" width="329" height="74" />][10]

点击&#8221;KiwiVM Control Panel&#8221;（如下图），进入服务器管理界面。

[<img class="alignnone size-full wp-image-43" src="http://prdwb.github.io/images/2015/05/Image-021.png" alt="Image 021" width="166" height="47" />][11]

点击左边栏最下面的选项&#8221;Shadowsocks Sever&#8221;。

[<img class="alignnone size-full wp-image-44" src="http://prdwb.github.io/images/2015/05/Image-022.png" alt="Image 022" width="237" height="168" />][12]

点击Install Shadowsocks Server。静等安装完成。

[<img class="alignnone size-full wp-image-45" src="http://prdwb.github.io/images/2015/05/Image-023.png" alt="Image 023" width="628" height="253" />][13]

看到绿色的&#8221;Completed&#8221;后，点击&#8221;Go Back&#8221;。

[<img class="alignnone size-full wp-image-46" src="http://prdwb.github.io/images/2015/05/Image-024.png" alt="Image 024" width="199" height="122" />][14]

可以看到Shadowsocks Server已经运行啦！

[<img class="alignnone size-full wp-image-47" src="http://prdwb.github.io/images/2015/05/Image-025.png" alt="Image 025" width="669" height="231" />][15]

下载Shadowsocks客户端并安装。

[<img class="alignnone size-full wp-image-48" src="http://prdwb.github.io/images/2015/05/Image-026.png" alt="Image 026" width="899" height="183" />][16]

按照页面中的图片配置服务器（黄色区域可以直接复制）。

[<img class="alignnone size-full wp-image-49" src="http://prdwb.github.io/images/2015/05/Image-027.png" alt="Image 027" width="517" height="452" />][17]

如果你想在浏览器中使用代理的话，可以参照下图，在此不赘述了。

[<img class="alignnone size-full wp-image-50" src="http://prdwb.github.io/images/2015/05/Image-028.png" alt="Image 028" width="1070" height="310" />][18]

至此，你就拥有了自己的代理服务器啦！

 [1]: http://prdwb.github.io/images/2015/05/Image-008.png
 [2]: http://prdwb.github.io/images/2015/05/Image-012.png
 [3]: http://prdwb.github.io/images/2015/05/Image-013.png
 [4]: http://prdwb.github.io/images/2015/05/Image-014.png
 [5]: http://prdwb.github.io/images/2015/05/Image-015.png
 [6]: http://prdwb.github.io/images/2015/05/Image-016.png
 [7]: http://prdwb.github.io/images/2015/05/Image-017.png
 [8]: http://prdwb.github.io/images/2015/05/Image-018.png
 [9]: http://prdwb.github.io/images/2015/05/Image-019.png
 [10]: http://prdwb.github.io/images/2015/05/Image-020.png
 [11]: http://prdwb.github.io/images/2015/05/Image-021.png
 [12]: http://prdwb.github.io/images/2015/05/Image-022.png
 [13]: http://prdwb.github.io/images/2015/05/Image-023.png
 [14]: http://prdwb.github.io/images/2015/05/Image-024.png
 [15]: http://prdwb.github.io/images/2015/05/Image-025.png
 [16]: http://prdwb.github.io/images/2015/05/Image-026.png
 [17]: http://prdwb.github.io/images/2015/05/Image-027.png
 [18]: http://prdwb.github.io/images/2015/05/Image-028.png