---
title: "VSCode 在 MacOS 中的 code 命令行"
date: 2025-06-12T20:30:00+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: false
tags: ["开发", "Shell"]
categories: ["开发"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20250612-tech-mac-code"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

在较新版本的 MacOS 系统中，VSCode 安装时不再默认注册 `code` 到命令行 `PATH` 环境变量之中。而根据 VSCode 的官网指南，官方提供了两种方式注册 `code` 命令。

[方式一](https://code.visualstudio.com/docs/setup/mac#_configure-the-path-with-vs-code)为通过命令面板的形式。这种方式的主要原理是在 `usr\local\bin` 下建立一个 link 文件，**然而经过测试，在使用 iTerm 的情况下，经常会遇到机器重启失效的情况，所以方式一并不推荐**。

[方式二](https://code.visualstudio.com/docs/setup/mac#_manually-configure-the-path)为主动写入 shell profile 的方式，经过测试，本方式较前者稳定很多，更为推荐。

### 参考连接

- [Visual Studio Code on macOS](https://code.visualstudio.com/docs/setup/mac)
- [StackOverflow - "code ." is not working in on the command line for Visual Studio Code on OS X/Mac](https://stackoverflow.com/questions/29955500/code-is-not-working-in-on-the-command-line-for-visual-studio-code-on-os-x-ma?__cf_chl_rt_tk=SsXg.vU9.HMjFwJ6QCUO2fZ7tsvLU0lqehzV2MGTYGM-1749640985-1.0.1.1-HxgpTctXOdg_JYLu3s.4tY3BXX_n4SDxEaO8vXD66Z4)

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
