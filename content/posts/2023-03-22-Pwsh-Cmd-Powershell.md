---
title: "在调用层次为 Pwsh -> Cmd -> Powershell 时的一些注意事项"
date: 2023-03-22T17:30:30+08:00
subtitle: "通过其他程序工具间接调用 Powershell 模块时..."  # can be deleted
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["Pwsh"]
categories: ["命令行"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20230322-pwsh-cmd-powershell"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

# 1. Background

新版 Powershell(Pwsh) 在很多方面都可以代替旧版 Powershell 而存在，然而在某些极为特殊的情况下，我们依旧可能面临到一些“奇特”的问题。

Powershell 由于具备很多强大的功能，所以它自己本身也是可以通过命令行的方式被其他 Shell（例如 Cmd）来进行调用的。但由于一些 Powershell 命令分属于不同的 Powershell 模块内，在某些情况下，命令的执行可能会出现例如：
```
The 'Get-ExecutionPolicy' command was found in the module 'Microsoft.PowerShell.Security', but the module could not be loaded. For more information, run 'Import-Module Microsoft.PowerShell.Security'.
```
的执行报错。

# 2. Reason

这种情况一般发生于调用层级关系为“Pwsh -> Cmd -> Powershell”的时候。

在 Windows 平台上，众多的编程程序调用命令行的方式均为通过 Cmd 来进行，例如 Python、Perl 等。若我们通过 Pwsh 来调用这些编写好的 Python、Perl 等程序，且我们在 Python、Perl 程序内调用了 `powershell` 相关的命令，这便达成了“Pwsh -> Cmd -> Powershell”的调用链。在这种情况下，Pwsh 的 module path 会被传递给 Powershell，导致了问题的出现。

# 3. Solve

解决办法也很简单，要么在编写 Python、Perl 等程序时，显式调用 Pwsh 而非 Powershell，要么在调用 Powershell 前，手动将 module path 进行重置（`set PSModulePath=`），例如：

```shell
set PSModulePath= && powershell Get-ExecutionPolicy
```

# References

* [[Regression] Can't import Security module when calling PowerShell from Python](https://github.com/PowerShell/PowerShell/issues/18681)
* [Cert:\ PSDrive is unavailable on PowerShell 5.1 launched by cmd when cmd is launched on PowerShell 7.3](https://github.com/PowerShell/PowerShell/issues/18530#issuecomment-1325691850)


---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
