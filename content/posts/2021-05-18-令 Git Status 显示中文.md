---
title: "令 Git Status 显示中文"
date: 2021-05-18T09:56:30+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["git", "CSDN备份"]
categories: ["git"]
series: ["tech"]
slug: "20210518-tech-git-status-cn"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

## 1. 问题描述

在 Windows 上使用 Git 时，默认情况下，`git status` 命令并不会显示中文的文件名（如下图所示）：
![默认情况的 Git Status](/img/posts/20210518094639325.png "默认情况的 Git Status")
**而我们所期望的是令其显示中文的文件名。**


## 2. 解决方案

在命令行键入命令：
```shell
git config --global core.quotepath false
```

即全局配置 Git 不对非英文字符转码

> [参考 Git 官方文档](https://git-scm.com/docs/git-config#Documentation/git-config.txt-corequotePath)

修改之后的 `git status` 效果如下所示：
![显示中文](/img/posts/20210518095009948.png "显示中文")


## 3. 注意事项

**为了确保中文显示不乱码，请使用支持 UTF-8 的终端操作 Git**（例如使用 Pwsh 的 Windows Terminal）。

以 Powershell 为例，可以通过配置启动配置文件来强制使用代码页 65001 来实现（即 `chcp 65001`）

> 关于配置 Powershell 启动文件，可参考[PSReadLine - Powershell 的强化工具：使用配置文件激活 PSReadLine Options](/posts/20220304-tech-pw-readline)


---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
