---
author: Yan Wang
date: 2018-03-31 11:37:52 -0700
layout: post
title: 视频监控系统小调查
categories: [小调查]
comments: true
tags:
-  调查
---

调查目标：找一套合适的软件系统，可以用于视频监控，并且部署在自己的硬件上。

硬性需求：
* 可以完全部署在自己的硬件上（电脑或者 NAS），不需要云端服务器介入。
* 可以支持多种网络摄像头（而不只是局限于几个特定品牌）。

其他关注点：

* 软件属性：是否开源，费用（一次性付费或者订阅模式，每个通道的价格），支持哪些部署环境。
* 监控系统特性：支持的通道数量，视频质量，是否支持智能识别（如运动捕获、人脸识别等），是否通过网页进行操作，是否有其他使用限制（比如免费版有水印或者不能长时间保存视频等）。

一些现成的列表：

* [TOP BEST FREE/PAID VIDEO MANAGEMENT SOFTWARE FOR IP CAMERAS](https://www.unifore.net/ip-video-surveillance/top-best-free-paid-video-management-software-for-ip-cameras.html)
* [CCTV Test: VMS for free?](http://benchmarkmagazine.com/cctv-test-vms-free/)


# 威联通 NAS 官方应用

作为威联通 NAS 的用户，首先想到的是威联通官方提供的 [QVR Pro](https://www.qnap.com/solution/qvr-pro-official/en/) 和 [Surveillance Station](https://www.qnap.com/solution/surveillance-station-intro/en/) 两个应用，虽然没试用过，也不知道这两者有何异同，不过相信官方应可维护性和性能应该比较好，而且给出了所有支持的网络摄像头[列表](https://www.qnap.com/en/compatibility-qvr-pro)，兼容性上应该不成问题。

主要问题就是贵，付费模式是一次性购买授权，Surveillance Station 的价格是每个通道 $60（最多4个通道），QVR Pro 的价格是每个通道 $60~70（最多8个通道）。不知道实际上需要多少通道，不过 4 个实在有点少，8 个还凑合。

因此，才会想要调查其他的可能选项，运行在自己的 NAS 上。

# [ZoneMinder](https://zoneminder.com)

一个开源免费的系统 ([Github](https://github.com/ZoneMinder/zoneminder))，根据介绍，这套系统提供网页操作支持，兼容网络摄像头（IP-enabled camera），给主流 Linux 发行版准备了安装包，也可以自己编译打包成 Docker 镜像，不支持 Windows。有[文档](https://zoneminder.readthedocs.io/en/stable/)介绍安装、部署、使用，有相对较活跃的[论坛](https://forums.zoneminder.com/)，甚至接通了 BountySource，可以付费悬赏实现特定的功能或者文档需求。

# [Rapidvms](http://veyesys.com/)

一个[开源](https://github.com/linkingvision/rapidvms)的系统（家用免费，商用付费），可以自己编译部署服务器端，有[文档](https://linkingv.gitbooks.io/rapidvmsusermanual/content/)支持。客户端声称跨平台，免费版是用特定的客户端软件进行操作，也可以按年付费（$30/年）使用该公司开发的 H5stream 系统，这样就可以使用网页版的控制台。似乎整个产品还在开发中，刚支持 OpenCV 并提供了一个人脸识别的用例，

# [Luxriot Evo](https://www.luxriot.com/product/ip-camera-software/luxriot-evo/)

