---
title: "Claude Desktop / Cowork 配置 DeepSeekV4 API 备忘记录"
subtitle: "UPDATE: 最新版本 Cowork 已失效..."
date: 2026-04-29T23:18:54+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["AI", "开发"]
categories: ["AI"]
series: ["tech"]
slug: "20260429-tech-claude-desktop-or-cowork-configure-deepseek-v4-notes"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

## **Update 2026.5.8:**

Claude Desktop 于近日发布了新版本，在配置 Gateway 接入 3P 模型时，**客户端会对模型名称做白名单筛查，这项修改使得所有非 Anthropic/Claude 的第三方模型接入直接失效**（除非配置反向代理来伪装模型名，才可以 Bypass 这个限制）。

这项更新，**其实变相官宣了 A ➗ 在官方意愿上禁用第三方模型接入自家产品**。不过目前 Claude Code CLI / VSC Ext 仍然可以接入第三方模型，**暂时**未受影响。

Coding Agent 有很多，但公认好用的也只有 CodeX 和 Claude Code……\
先是 CodeX 禁用了 wire_api 配置，然后是 Claude Cowork 对模型名做白名单筛查，**下一个会不会轮到 Claude Code 禁止第三方模型接入，可能真的不好说了**……

---

## Step 1. 下载 Claude Desktop

前往 [Claude Code Docs - Use Claude Code Desktop](https://code.claude.com/docs/en/desktop) 选择对应平台下载即可。

> 注意：可能需要全局上网，包括后续所有的步骤

## Step 2. 启用开发者模式

通过菜单启用开发者模式：

![开启 Claude Desktop 的开发者模式](/img/posts/20260429-c1.jpg "开启 Claude Desktop 的开发者模式")

之后前往第三方模型的配置页面：

![前往第三方模型的配置页面](/img/posts/20260429-c2.jpg "前往第三方模型的配置页面")

## Step 3. 配置 Claude Desktop

首先配置第三方模型的 Base URL 和 API Key：

![配置 Base URL 和 API Key](/img/posts/20260429-c3.jpg "配置 Base URL 和 API Key")

然后配置模型名称，并打开 1M-context 的选项开关，之后跳过 Login 页面：

![配置模型名称（带 1m 参数选项开关）并跳过登录](/img/posts/20260429-c4.jpg "配置模型名称（带 1m 参数选项开关）并跳过登录")

接下来允许 Web Search 任意网页：

![允许 Web Search 任意网页](/img/posts/20260429-c5.jpg "允许 Web Search 任意网页")

最后禁用 Telemetry 并应用更改：

![禁用 Telemetry](/img/posts/20260429-c6.jpg "禁用 Telemetry")

## References

- [Claude Support - Install and configure Claude Cowork with third-party platforms](https://support.claude.com/en/articles/14680741-install-and-configure-claude-cowork-with-third-party-platforms#h_c00b8c02e0)
- [Claude Docs - Cowork on 3P - Configuration Reference](https://claude.com/docs/cowork/3p/configuration)

## Trouble Shooting Guide

### TSG 1. Windows 所需的额外配置

Claude Cowork 在 Windows 上需要借助于 Linux 虚拟环境来运行 Bash 命令，可按如下流程开启 Windows 的虚拟机平台功能：

![开启 Windows 的虚拟机平台功能](/img/posts/20260429-c7.jpg "开启 Windows 的虚拟机平台功能")

### TSG 2. `Workspace still starting. The isolated Linux env...`

接续 TSG 1，这也是 Windows 平台可能遇到的特定问题。在运行 Bash Tool 时，可能会出现：

```
Workspace still starting. The isolated Linux environment is booting in the background (usually 10–30 seconds). Try again shortly
```

参考[此链接](https://linux.do/t/topic/2072551)，解决办法如下：

> 使用管理员方式运行claude desktop后等待2-3分钟下载安装好cowork所需的底层服务（此时你可以打开任务管理器->性能->Wi-Fi 查看网速是否正在下载/下载完成）
>
> 测试了多台windows 和 一台mac，其中包含一台没有任何开发环境（WSL/hyper-v/python等都没有）也解决了这个问题，并且可以正常让claude desktop执行cmd和python命令

### TSG 3. DeepSeek 模型名显示存在 `*` 号

**这属于 Claude Desktop 模型名称显示的 BUG，与模型名字符串的长短没有任何关系**。Claude Desktop 在渲染如 `{英文}-v{数字}-{英文}` 的模型字符串时，均会触发此 BUG，可以用任意虚假的模型名（如：`fifa-v7-nba`）来验证这一点：

![Claude Desktop 渲染 {英文}-v{数字}-{英文} 模型字符串的 BUG](/img/posts/20260429-c8.jpg "Claude Desktop 渲染 {英文}-v{数字}-{英文} 模型字符串的 BUG")

## BTW

额外多说几句，**Claude Desktop 的可配置选项相比 Claude Code 少太多了，这也导致其能力也会受到一定程度影响**（例如在配置第三方模型时，SubAgent 的 Model 只能继承主 Agent，无法指定其它 Model 来经济化运行）。

*同样都是“号称 100% AI Coding”的产品*，**Claude Code 的 BUG 已经不少了，没想到 Claude Desktop / Cowork 的 BUG 更多**（还有 Claude Cowork 手动加载 `.zip` SKILL 时加载不上 `scripts/` 文件夹等 BUGs）……

***如果 A ÷ 继续在 LLM 参数量“力大砖飞”的路上一去不复返，那么 A ÷ 的好日子也就真的快到头了……***

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
