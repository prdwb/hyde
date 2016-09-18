---
layout: post
title: 视频搬运服务
---

已经半年多没有更新博文了，这次更新是因为写了一个有意思的东西，是一个视频搬运的服务，可以定时爬取YouTube上指定Channel的视频，将新视频下载下来，再上传到优酷。这个服务分为三个部分，VideoCrawler，VideoDownloader和VideoUploader。这样不挂代理，就可以看到那些我喜欢的YouTuber上传的最新视频啦。

## VideoCrawler
这个是一个基于Scrapy的爬虫，可以爬取我喜欢的Channel的视频列表，将视频名字、描述、URL等等信息存到数据库中，方便后续的视频下载。我利用Crontab定期执行这个爬虫，爬取增量信息，也就是新上传的视频。增量的部分是这样处理的：表中url字段加上唯一性约束（unique index），然后插入时用 `insert ignore into` 。

## VideoDownloader
查询数据库，找到 `download_finished = 0` 的URL，依次下载，文件用数据库中的 `id` 命名。下载这里用的是 [PyTube](https://github.com/nficano/pytube)。

## VideoUploader
查询数据库，找到 `download_finished = 1 and upload_finished = 0` 的文件名，依次上传，上传使用的是优酷提供的API，选优酷的原因就是它是唯一一个提供API的视频网站，上传这个地方我也找到了别人写好的模块，叫[youku](https://github.com/hanguokai/youku)。

## 三者的联系
这三个模块互相没有调用的关系，VideoCrawler在每天自动执行一次，很快就可以爬完增量信息，而VideoDownloader和VideoUploader是启动后就一直执行，如果数据库中没有符合条件的，就休眠一小会。这次这个服务是部署在腾讯云上的那个学生主机，由于带宽有限，而且下载还要走代理，所以下载速度异常的慢，上传速度还好。


项目地址：[TransVideos](https://github.com/prdwb/TransVideos)
