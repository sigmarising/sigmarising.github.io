---
title: "PotPlayer + LAV + MadVR + XySubFilter 配置指南（2025版）"
date: 2025-01-15T19:20:45+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["效率", "软件"]
categories: ["软件"]
series: ["tech"]  
slug: "20250115-tech-potplayer-lav-madvr-xysubfilter"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---


# Introduction

Potplayer 作为 Windows 上很受欢迎的一款播放器，公认的最佳配置为结合 LAV、madVR、XySubfilter 进行使用。本文将介绍一种对其简单配置的方法。
* LAV：一套主流的开源解码 filter
* MadVR：一个高性能的视频渲染器
* XySubFilter：madVR 专用的字幕插件

# Configuration

注：本文默认机器使用 64 位系统。

## Step 1. 显卡设置

第一步的设置即为配置显卡驱动控制面板的显示设置，针对于 Intel、Nvidia、AMD 的显卡配置方法各不相同，可以参考[此链接](https://vcb-s.com/archives/7228)或其他资料并结合自身机器的显卡和显示器进行配置，本文不再赘述。

## Step 2. 安装 PotPlayer、LAV、MadVR、XySubFilter

自行下载以下安装包：
* [PotPlayer](https://potplayer.daum.net/?lang=zh_CN) 64bit 版本
* [LAV Filters](https://github.com/Nevcairiel/LAVFilters/releases)
* [madVR](https://forum.doom9.org/showthread.php?t=146228)
* [XySubFilter](https://github.com/pinterf/xy-VSFilter/releases)

说明：
* PotPlayer 下载地址为官方网站
* LAV Filters 下载地址为 GitHub 发布页
* madVR 下载地址为 Doom9 论坛，原因参考此[Reddit 链接](https://www.reddit.com/r/software/comments/1ai1yxy/where_to_download_official_madvr_filesinstaller/?rdt=50717)
* XySubFilter 下载地址为基于原版修改的 pfmod 版本的 GitHub 发布页

### 为什么不再推荐使用 K-Lite ?

在我先前的配置文章[PotPlayer + LAV + MadVR + XySubFilter 配置指南（修订版）](/posts/20230824-tech-pro-pot-player)中，都是使用 K-Lite 打包安装 LAV + MadVR + XySubFilter 的。但是由于 K-Lite 在不断的更新中，存在以下的几个严重缺点，所以**我不再推荐使用 K-Lite 打包安装解码器**：
* 添加了在安装步骤中的捆绑软件选项
* 额外安装了很多不需要的解码器和渲染器
* 自 v18 开始移除了 XySubFilter 插件（参考此[链接](https://zh.wikipedia.org/zh-cn/K-Lite_Codec_Pack#:~:text=%E8%87%AA18.0.0%E8%B5%B7%EF%BC%8CXySubFilter%E8%A2%AB%E7%A7%BB%E9%99%A4%E3%80%82)）

### Step 2.1 安装 PotPlayer

安装 PotPlayer 时，需注意以下的选项\
这里需要**关联所有格式**，我还取消了创建快捷方式和快速启动栏图标：

![关联所有格式](/img/posts/20230824-p1.png "关联所有格式")

由于接下来要安装 K-Lite，额外的编解码器不需要再装；同时可以检测一下检测硬件编解码器：

![检测硬件编解码器](/img/posts/20230824-p2.png "检测硬件编解码器")

### Step 2.2 安装 LAV、MadVR、XySubFilter

首先需要下载好所有程序的压缩包，并手动解压、拷贝到目标安装目录，后续不再赘述。

#### Step 2.2.1 安装 LAV Filters

**注：下载时选择 LAVFilters-x.x.x-x64.zip 版本的压缩包。**

对目录中的三个 Install Bat 脚本，依次使用管理员权限运行，即可完成安装（安装完成后点击确认即可）：

![LAV Filters 的三个安装脚本](/img/posts/20250115-p1.png "LAV Filters 的三个安装脚本")

#### Step 2.2.2 安装 MadVR

对目录中的 Install Bat 脚本，使用管理员权限运行，即可完成安装（安装完成后按回车即可）：

![MadVR 安装脚本](/img/posts/20250115-p2.png "MadVR 安装脚本")

#### Step 2.2.3 安装 XySubFilter

进入 `x64` 文件夹，对目录中的 Install Bat 脚本，使用管理员权限运行，即可完成安装（安装完成后点击确认即可）：

![XySubFilter 安装脚本](/img/posts/20250115-p3.png "XySubFilter 安装脚本")

## Step 3. 配置 PotPlayer 滤镜选项

打开 PotPlayer 并打开设置，首先关闭内置图像处理滤镜：

![关闭内置图像处理滤镜](/img/posts/20230824-p0.png "关闭内置图像处理滤镜")

**其中 PotPlayer 内置图像滤镜必须关闭**，否则数据传递给 MadVR 时已经从 10bit 降低到了 8bit，精度会造成损失。

**而“内置声音处理滤镜设置”中的所有选项也可以都勾选上**。其中*内置声音处理滤镜可以在变速播放时保持音调不变*，*内置声音编码器/滤镜音频流切换可以避免（有外挂音轨存在时）内封与外挂音轨同时播放*。

接下来定位到全局滤镜，添加系统滤镜：

![添加系统滤镜](/img/posts/20230824-p10.png "添加系统滤镜")

添加如下所示：

![添加的系统滤镜](/img/posts/20230824-p11.png "添加的系统滤镜")

**对每个滤镜设置强制使用**：

![强制使用滤镜](/img/posts/20230824-p12.png "强制使用滤镜")

设置使用内置 WASAPI 音频渲染器：

![使用内置 WASAPI](/img/posts/20230824-p13.png "使用内置 WASAPI")

打开一个视频，按 `TAB` 查看信息可发现配置成功：

![最终效果](/img/posts/20250115-p4.png "最终效果")

## Step 4. 其他 PotPlayer 设置

### Step 4.1 播放设置

自动加载外部音频，开启预览窗格：

![播放设置 1](/img/posts/20230824-p15.png "播放设置 1")

不以关键帧为时间跨度移动：

![播放设置 2](/img/posts/20230824-p16.png "播放设置 2")

不记忆播放列表：

![播放设置 3](/img/posts/20230824-p17.png "播放设置 3")

### Step 4.2 消息和鼠标操作

使用你喜爱的字体显示消息：

![消息设置](/img/posts/20230824-p18.png "消息设置")

**为适应大部分人的习惯，设置鼠标单击为播放/暂停，双击为全屏/还原**：

![鼠标设置](/img/posts/20230824-p19.png "鼠标设置")

### Step 4.3 调整视频色彩空间/属性

将 **YCbCr<->RGB 规则** 调整为 **自动选择**：

![视频色彩空间设置](/img/posts/20230824-p20.png "视频色彩空间设置")

### Step 4.4 关闭音频规格化

![关闭音频规格化](/img/posts/20230824-p21.png "关闭音频规格化")

### Step 4.5 调整字幕选项

在一般情况下，需要关闭 PotPlayer 默认字幕，防止出现双行字幕。**外挂字幕文件可以通过“同路径下相同文件名”的方式被自动载入。**

但是 **xy-SubFilter 无法渲染图形外挂字幕 PGS 与 SUP，这些字幕仍然需要开启 PotPlayer 默认字幕进行渲染。**

值得注意的是，**使用 xy-SubFilter 之后，你将无法使用拖拽字幕文件到 PotPlayer 的方式来让 xy-SubFilter 加载字幕。如果 你对拖拽字幕功能需求强烈，你仍然需要使用 PotPlayer 默认字幕进行渲染。**

关闭 PotPlayer 默认字幕的位置：

![关闭 PotPlayer 默认字幕](/img/posts/20230824-p22.png "关闭 PotPlayer 默认字幕")

关于 xy-SubFilter 的设置，根据自身需求选择:

![xy-SubFilter 设置](/img/posts/20230824-p23.png "xy-SubFilter 设置")

### Step 4.6 LAV 设置

LAV Splitter 和 LAV Splitter Source 无需配置，保持默认即可。

配置 LAV Video Decoder：

![配置 LAV Video Decoder](/img/posts/20230824-p24.png "配置 LAV Video Decoder")

确认输出格式如下：

![确认输出格式](/img/posts/20230824-p25.png "确认输出格式")

配置 LAV Audio Decoder：

![配置 LAV Audio Decoder](/img/posts/20230824-p26.png "配置 LAV Audio Decoder")

确认开启 Mixing：

![确认开启 Mixing](/img/posts/20230824-p27.png "确认开启 Mixing")

### Step 4.7 MadVR 设置

进入设置界面：

![设置 MadVR](/img/posts/20230824-p28.png "设置 MadVR")

设置对应显示器的类型：

![设置显示器类型](/img/posts/20230824-p29.png "设置显示器类型")

**在 properties 选项中，外接电视选 TV levels (16-235)，电脑显示器选 PC levels（0-255）。**

**如果是 8bit 显示器就选 8bit 或者 7bit，10bit 显示器选10bit or higher（或者选择 auto）。**

![设置显示器 properties](/img/posts/20230824-p30.png "设置显示器 properties")

***关于其他 LAV 和 madVR 的高级设置，请参阅参考链接***

# 参考链接

* [高品质视频播放器 Potplayer + madvr + LAV 设置不完全指北【2024-01-24更新】](https://bbs.pha.pub/threads/286/)
* [基于 PotPlayer 和 madVR 的播放器教程](https://vcb-s.com/archives/7228)
* [配置全设备通用的 PotPlayer 和 LAVFilters 满足基本 BDRIP 回放需求](https://blog.dsrkafuu.net/post/2020/potplayer-with-lav-fliters/)
* [Potplayer 安装 LAV Filters 改善播放体验与质量](https://blog.nannan.cool/archives/256/)
* [Potplayer+Lav Filters+madVR](https://zhuanlan.zhihu.com/p/33615747)

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
