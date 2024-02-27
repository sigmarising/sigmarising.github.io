---
title: "关于 Window 的 UWP 应用本地回环限制以及限制解除方案"
date: 2022-01-31T11:27:58+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["开发", "CSDN备份"]
categories: ["开发"]
series: ["tech"]
slug: "20220131-tech-uwp-loopback"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

## 1. UWP 应用默认禁止本地回环

UWP 应用在默认的情况下，禁止访问本地的 Localhost（这被称做本地回环 Loopback）。

然而我们在开发、调试、正向代理等场景下，又不得不让 UWP 突破这一个限制。

> 参考链接：
> * [MS Docs - App architecture](https://docs.microsoft.com/en-us/previous-versions/windows/apps/dn640582(v=win.10)?redirectedfrom=MSDN#app-architecture)
> * [MS Docs - Deploying and debugging UWP apps](https://docs.microsoft.com/en-us/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#debugging-options)


## 2. 官方限制解除工具 `CheckNetIsolation.exe`

这个工具位于 `C:/Windows/System32/CheckNetIsolation.exe`，它的功能为解除 UWP 的本地 Loopback 限制或者调试应用（本文仅介绍解除 Loopback 限制）。

```shell
./CheckNetIsolation.exe LoopbackExempt [operation] [-n=] [-p=]
```

常见用法：
* `-s`: 查看已经取得 Loopback 豁免的应用列表
* `-a -p=[App Container SID]` or `-a -n=[App Container Name]`: 添加应用豁免
* `-d -p=[App Container SID]` or `-d -n=[App Container Name]`: 移除应用豁免
* `-c`: 移除所有安装的应用的豁免

> 参考链接：
> * [MS Docs - Enable loopback for network access](https://docs.microsoft.com/en-us/previous-versions/windows/apps/hh780593(v=win.10)?redirectedfrom=MSDN#enable-loopback-for-network-access)
> * [MS Docs - Enabling loopback for a UWP application](https://docs.microsoft.com/en-us/windows/iot-core/develop-your-app/loopback#enabling-loopback-for-a-uwp-application)

## 3. 如何获取所有安装应用的 SID

在注册表目录 `HKCU\Software\Classes\Local Settings\Software\Microsoft\Windows\CurrentVersion\AppContainer\Mappings` 即可取得所有 App 的 SID 列表。

通过结合 CMD 或者 Pwsh 的循环命令，即可自动化为所有已安装应用添加豁免：

CMD 命令：
```shell
FOR /F "tokens=11 delims=\" %p IN ('REG QUERY "HKCU\Software\Classes\Local Settings\Software\Microsoft\Windows\CurrentVersion\AppContainer\Mappings"') DO CheckNetIsolation.exe LoopbackExempt -a -p=%p
```

Powershell 命令：
```shell
Get-ChildItem -Path Registry::"HKCU\Software\Classes\Local Settings\Software\Microsoft\Windows\CurrentVersion\AppContainer\Mappings\" -name | ForEach-Object {CheckNetIsolation.exe LoopbackExempt -a -p="$_"}
```

New Powershell Core:
```shell
(Get-AppxPackage -AllUsers).PackageFamilyName | ForEach-Object {CheckNetIsolation.exe LoopbackExempt -a -n="$_"}
```

> 参考链接：
> * [一般方法 - Windows 用户：UWP 应用回环问题](https://qv2ray.net/lang/zh/getting-started/step4.html#%E4%B8%80%E8%88%AC%E6%96%B9%E6%B3%95)
> * [MS Learn - Loopback](https://learn.microsoft.com/en-us/windows/uwp/communication/interprocess-communication#loopback)

## 4. 其他便捷工具以及深层次分析

来自 Fiddler 的 [Enable Loopback Utility](https://telerik-fiddler.s3.amazonaws.com/fiddler/addons/enableloopbackutility.exe) 或开源项目 [Loopback Exemption Manager](https://github.com/tiagonmas/Windows-Loopback-Exemption-Manager) 的这两个工具提供了很方便的图形化方式来对安装的 UWP 应用进行 Loopback 豁免。

深入研究[后者工具](https://github.com/tiagonmas/Windows-Loopback-Exemption-Manager/blob/master/Loopback/Loopback.cs)以及[类似工具](https://github.com/voidregreso/EnableUWPLoopback/blob/master/IsolationAPI/LoopbackIsolation.cs)的源代码，并参考作者提到的：

> Thanks to [Eric Lawrence](http://stackoverflow.com/users/126229/ericlaw) for helping with the PInvokes.
>
> 注：提到的此人为 Fiddler 的作者

我们可以知道，开源工具的作者从 Fiddler 那里借鉴了 P/Invokes 的方式获取 API，而所 Invoke 的 API 主要来自于 `FirewallAPI.dll`，这个 dll 提供了很多与 UWP Loopback 豁免相关的实用方法。

我们继续使用 VS2022 的 Dev Shell 对官方的 `CheckNetIsolation.exe` 进行 dll 依赖分析（需要安装 VS 2022）：
![依赖 dll 分析](/img/posts/bfc014ce1aeb47c5b3edb1d6c181aed2.png "依赖 dll 分析")
到这里我们便可以得知，无论是官方工具还是第三方工具的实现，都是调用 `FirewallAPI.dll` 提供的 API 来进行 Loopback 豁免的。

> 参考链接：
> * [Stackoverflow - Can't see localhost from UWP app](https://stackoverflow.com/questions/34589522/cant-see-localhost-from-uwp-app?rq=1)
> * [MS Docs - Platform Invoke (P/Invoke)](https://docs.microsoft.com/en-us/dotnet/standard/native-interop/pinvoke)


> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
