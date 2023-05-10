---
title: "管理 PowerShell 的命令历史"
date: 2023-05-10T18:21:25+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["Shell"]
categories: ["Shell"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20230510-pwsh-history"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

## 介绍

PowerShell 的命令历史记录可以用来做很多事情，例如提供给 PSReadLine 来完成命令预测和历史搜索。

若想要管理这些本地的历史记录，只需要运行：
```shell
(Get-PSReadlineOption).HistorySavePath
```

此时命令行打印的路径即为历史命令的记录文件，使用文本编辑器对其进行编辑即可。

## 参考链接

[PowerShell: Clear History of Previous Commands](https://www.shellhacks.com/clear-history-powershell/)

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
