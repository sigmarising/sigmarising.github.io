---
title: "在 Windows 上安装新版本 gcc/g++"
date: 2020-07-11T20:22:09+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["开发", "CSDN备份"]
categories: ["开发"]
series: ["tech"]
slug: "20200711-tech-win-new-gcc"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

通过 MinGW Installation Manager 能够安装到的 gcc/g++ 版本并不会很新：
![mingw i m](/img/posts/20200711201231180.png "mingw install manager")

本文将会介绍安装新版本 gcc/g++ 的方法。


## Step1. 下载并安装 MinGW-W64

下载 [MinGW-W64](https://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win32/Personal%20Builds/mingw-builds/installer/mingw-w64-install.exe/download).

安装时注意选项如下：
![option](/img/posts/20200711201509266.png "option")

之后会联网进行下载安装。


## Step 2. 添加到 PATH

安装完毕后，将如下路径添加到系统的 PATH：
![PATH](/img/posts/2020071120164519.png "PATH")


## Step 3. 测试

如图所示，安装成功：
![test](/img/posts/20200711201736949.png "test")


## 参考链接

* [Visual Studio Code - Using GCC with MinGW](https://code.visualstudio.com/docs/cpp/config-mingw)
* [What is difference between sjlj vs dwarf vs seh?](https://stackoverflow.com/questions/15670169/what-is-difference-between-sjlj-vs-dwarf-vs-seh)
* [MingGW64 版本说明](https://www.pcyo.cn/linux/20181212/216.html)


---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
