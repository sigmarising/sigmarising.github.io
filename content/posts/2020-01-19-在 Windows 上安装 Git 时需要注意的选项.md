---
title: "在 Windows 上安装 Git 时需要注意的选项"
date: 2020-01-19T11:05:41+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["开发", "CSDN备份"]
categories: ["开发"]
series: ["tech"]
slug: "20200119-tech-win-git"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

在 [Git 官网下载页](https://git-scm.com/downloads) 可以下载到 Windows 版本的 Git 安装包。

在安装过程中，有以下两个选项需要注意。

## Notice 1. PATH Environment

![path 设置](/img/posts/20200119105519268.png "path 设置")

**如上图所示，这里选择第二个选项**。这样 Git 就可以在 cmd、powershell、IDE 的集成终端等多处地方使用了。

## Notice 2. Line Ending
![line ending](/img/posts/20200119105900348.png "line ending")

**在行尾设置时，我们选择第三项**。即 checkout 和 commit 时不改变行尾，否则自动的行尾更改可能会导致代码风格检查工具的大量报错。

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
