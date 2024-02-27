---
title: "设置 Pwsh 的 Proxy 地址"
date: 2024-02-27T21:09:38+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["Shell"]
categories: ["Shell"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20240227-tech-pwsh-proxy"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

## 设置方法

以新版 Powershell Core 即 Pwsh 为例，在命令行中设置当前会话代理 Proxy 的方法为（假设代理地址为 `http://127.0.0.1:12345`）：

```shell
$env:HTTP_PROXY="http://127.0.0.1:12345"; $env:HTTPS_PROXY="http://127.0.0.1:12345"
# or
$env:ALL_PROXY ="http://127.0.0.1:12345"
```

## 参考链接

* [Bypass proxy for all web requests in Powershell Core](https://superuser.com/questions/1649763/bypass-proxy-for-all-web-requests-in-powershell-core)
* [MS Learn - HttpClient.DefaultProxy Property](https://learn.microsoft.com/en-us/dotnet/api/system.net.http.httpclient.defaultproxy)
* [Use the Az PowerShell module behind a proxy](https://learn.microsoft.com/bs-latn-ba/powershell/azure/az-powershell-proxy?view=azps-11.3.0&viewFallbackFrom=azps-11.0.0)
* [Powershell命令行设置代理](https://blog.csdn.net/lxyoucan/article/details/127843271)

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
