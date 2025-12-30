---
title: "关于 Node Version Manager 的个人最佳实践"
date: 2025-12-30T15:07:10+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["前端", "开发"]
categories: ["开发"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20251230-tech-node-mgr"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

对于 Node Version 的管理是一名前端研发必然要面临的问题，而在我实践过长时间后，总结了一点点在 Win/Mac 上使用 Node Version Manager 的经验。

## Windows

由于 Windows 历史悠久的 Pwsh、PowerShell、cmd 混用问题，**在此强烈不推荐依赖于注入 shell profile 方式的 fnm 工具**，尤其是如果你经常在 Pwsh/PowerShell 和 cmd 间切换使用时。

相对的，在 Windows 上更推荐注入环境变量方式和 Link 方式的管理工具，例如：
- [nvs](https://github.com/jasongin/nvs)
- [nvm-windows](https://github.com/coreybutler/nvm-windows)

nvs 由一位在 Microsoft 工作的老哥主导开发，并且 nvs 也在某段时间作为了 MS 内部某些前端 Repo Environment Setup 工具的一环。但不知未何，nvs 已经年久失修（两年未更新），且其已被移出了 setup 工具链。

所以综上来看，目前在 Windows 上更为推荐的是还在活跃维护的 nvm-windows 工具（即便我个人认为 nvs 的使用方法更为优雅一些）。

## Posix（Mac、Linux、WSL）

在 Posix 上，依赖于 Shell Profile 注入的 nvm 和 fnm 都是不错的选择。而在今天来看，速度更快且好评更多的 [fnm](https://github.com/Schniz/fnm) 似乎是更优秀的选择。

## 总结

虽然在个人使用习惯上，我的偏好是 nvs > fnm ≈ nvm/nvm-windows，但实践中：
- Windows 请使用 nvs/nvm-windows，优先选用 nvm-windows
- Posix 请使用 nvm/fnm，优先选用 fnm

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
