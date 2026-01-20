---
title: "PotPlayer + MPCVR 极简配置指南"
date: 2026-01-20T08:49:05+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["效率", "软件"]
categories: ["效率", "软件"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20260120-tech-pot-player-mpcvr"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

## Introduction

本文不同于 [PotPlayer + MadVR 的详细配置指南](/posts/20250115-tech-potplayer-lav-madvr-xysubfilter/)，而是旨在最大化利用 PotPlayer 已有的内置能力 + MPCVR，以求更好的播放 MadVR 无法处理的杜比视界文件，达到较好的视频播放兼容效果。

## Configuration

**本文默认机器使用 64 位系统。**

### Step 1. 显卡设置

第一步的设置即为配置显卡驱动控制面板的显示设置，针对于 Intel、Nvidia、AMD 的显卡配置方法各不相同，可以参考[此链接](https://vcb-s.com/archives/7228)或其他资料并结合自身机器的显卡和显示器进行配置，本文不再赘述。

### Step 2. 安装 PotPlayer + OpenCodec + MPCVR

自行下载安装包：
- [PotPlayer 64 位版本](https://potplayer.tv/?lang=zh_CN)
- [MPCVR - Github Release Page](https://github.com/Aleksoid1978/VideoRenderer/releases)

#### Step 2.1 安装 PotPlayer + OpenCodec

安装 PotPlayer 时，需要注意**勾选关联所有格式**：

![关联所有格式](/img/posts/20260120-p1.jpg "关联所有格式")

同时，**安装额外的编解码器**并**检测硬件编解码器**：

![安装额外的编解码器并检测硬件编解码器](/img/posts/20260120-p2.jpg "安装额外的编解码器并检测硬件编解码器")

关于 OpenCodec 的安装，勾选全部组件：

![勾选全部组件](/img/posts/20260120-p3.jpg "勾选全部组件")

#### Step 2.2 安装 MPCVR

将 MPCVR 的安装包解压至安装目录（**后续不要移动或删除**），以管理员身份运行安装脚本即可：

![安装 MPCVR](/img/posts/20260120-p4.jpg "安装 MPCVR")

### Step3. 配置 PotPlayer

#### Step 3.1 滤镜设置

首先切换至滤镜选项，确保使用所有内部滤镜：

![使用所有内部滤镜](/img/posts/20260120-p5.jpg "使用所有内部滤镜")

内置 OpenCodec 选项，按照自身设备的显卡条件，按需进行选择（下图案例中我的显卡是 N 卡）：

![内置 OpenCodec 选项](/img/posts/20260120-p6.jpg "内置 OpenCodec 选项")

视频解码器选项，开启 D3D12 硬件加速：

![开启 D3D12 硬件加速](/img/posts/20260120-p7.jpg "开启 D3D12 硬件加速")

切换渲染器为 MPCVR：

![切换渲染器为 MPCVR](/img/posts/20260120-p8.jpg "切换渲染器为 MPCVR")

切换音频输出设备为内置 WASAPI：

![切换音频输出设备为内置 WASAPI](/img/posts/20260120-p9.jpg "切换音频输出设备为内置 WASAPI")

#### Step 3.2 其他设置

自动加载外部音频，开启预览窗格：

![自动加载外部音频，开启预览窗格](/img/posts/20260120-p10.jpg "自动加载外部音频，开启预览窗格")

不以关键帧为时间跨度移动：

![不以关键帧为时间跨度移动](/img/posts/20260120-p11.jpg "不以关键帧为时间跨度移动")

不记忆播放列表：

![不记忆播放列表](/img/posts/20260120-p12.jpg "不记忆播放列表")

使用你喜爱的字体显示消息：

![使用你喜爱的字体显示消息](/img/posts/20260120-p13.jpg "使用你喜爱的字体显示消息")

**为适应大部分人的习惯，设置鼠标单击为播放/暂停，双击为全屏/还原**：

![鼠标设置](/img/posts/20260120-p14.jpg "鼠标设置")

## 最终效果

![最终效果](/img/posts/20260120-p15.jpg "最终效果")

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
