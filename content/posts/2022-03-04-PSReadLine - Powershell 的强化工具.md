---
title: "PSReadLine - Powershell 的强化工具"
date: 2022-03-04T09:07:17+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["Shell", "CSDN备份"]
categories: ["Shell"]
series: ["tech"]
slug: "20220304-tech-pw-readline"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

UPDATE 2022.3.4: 根据其 [Github README](https://github.com/PowerShell/PSReadLine) 的说明，If you are using Windows PowerShell on Windows 10 or using PowerShell 6+, PSReadLine is already installed. 即使用最新版 Powershell Core 则会内置 PSReadLine。

[PSReadLine](https://github.com/PowerShell/PSReadLine) 在 Github 上属于 Powershell 官方组织库之下，是一款实用的增强 Powershell 的工具。

本文将简单介绍 PSReadLine 的几个强大功能，建议同时配合下述工具使用：
* [Powershell Core](https://github.com/PowerShell/PowerShell)
* [Windows Terminal](https://github.com/microsoft/terminal)

## 安装 PSReadLine

由于我们建议使用的是 Powershell Core，所以在 Powershell Core 下，用管理员身份启动，输入下面命令即可：

```shell
Install-Module -Name PSReadLine -AllowPrerelease -Scope CurrentUser -Force -SkipPublisherCheck
```

## 使用配置文件激活 PSReadLine Options
如果你之前没有创建过 Powershell Profile，使用下列命令创建：

```shell
if (!(Test-Path -Path $PROFILE )) { New-Item -Type File -Path $PROFILE -Force }
notepad $PROFILE
```

命令 `$PROFILE` 会显示配置文件的路径。

我的配置文件分享：
```ps1
chcp 65001

# PSReadLine
Import-Module PSReadLine
# Enable Prediction History
Set-PSReadLineOption -PredictionSource History
# Advanced Autocompletion for arrow keys
Set-PSReadlineKeyHandler -Key UpArrow -Function HistorySearchBackward
Set-PSReadlineKeyHandler -Key DownArrow -Function HistorySearchForward
```

我的配置文件首先切换到 UTF-8 代码页，之后导入模块 PSReadLine，并设置依据历史进行命令补全预测、以及高级的方向键历史命令搜索。

## 自定义配置

键入命令：
```shell
Get-PSReadLineOption  # 显示所有可以配置的选项
Get-PSReadLineKeyHandler  # 显示所有可以配置的快捷键
```

之后在配置文件中通过 `Set-PSReadLineOption` 和 `Set-PSReadlineKeyHandler` 可以进行配置。

## 实用效果

预测历史命令：
![预测历史命令](/img/posts/20200711171032190.png "预测历史命令")

使用 `Ctrl + Space` 补全命令（此快捷键与微软拼音输入法冲突，建议取消输入法对应的快捷键）：
![补全](/img/posts/20200711171158409.png "补全")

高级方向键搜索历史命令（方向键上或者下）：
![搜索](/img/posts/20200711171348398.png "搜索")


---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
