---
title: "在 Linux 上使用 screen 命令在后台运行程序"
date: 2019-05-09T15:26:52+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["Linux", "CSDN备份"]
categories: ["Linux"]
series: ["tech"]
slug: "20190509-tech-screen"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

我们使用 ssh 登录到 Linux 服务器上时，可能会执行一些需要长时间运行的程序，这时候我们便可以使用 screen 命令了。

## 场景分析

我们需要运行一个深度学习任务，需要 GPU 服务器运行 30 小时。

我们的操作步骤如下：
* ssh 登录到 GPU 服务器
* 配置你的 venv/conda 环境
* 传输代码文件
* 执行命令 `screen -S <name>`
* 在新的会话中激活 python 的 venv/conda 环境
* 运行深度学习任务
* 使用快捷键 `Ctrl + A + D` 暂时离开会话
* 已经可以断开 ssh 连接了，下一次登录后，使用 `screen -r <name>` 恢复会话

可以使用 `screen -ls` 查看所有的会话

## 参考链接

* [linux screen的用法](https://www.jianshu.com/p/e91746ef4058)
* [screen inside the conda environment doesnt work](https://stackoverflow.com/questions/50591901/screen-inside-the-conda-environment-doesnt-work)
* [screen命令](http://man.linuxde.net/screen)


---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
