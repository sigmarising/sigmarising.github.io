---
title: "VSCode Github Copilot 直接通过 BYOK 接入 DeepSeek V4"
date: 2026-06-06T14:52:47+08:00
subtitle: "无需安装第三方扩展，直接利用 VSCode 第一方特性接入第三方模型"  # can be deleted
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["AI", "开发"]
categories: ["AI"]
series: ["tech"] 
slug: "20260606-tech-vscode-github-copilot-byok-with-deepseek"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

## Introduction

VSCode 近日正式版的更新中，第一方特性支持了通过 BYOK 的形式接入第三方模型到 Github Copilot Chat 中。这种方式最大的好处就是**零第三方插件依赖，无需安装任何其他扩展，直接使用官方特性**。本文介绍按此方法接入的简单方法。

## Step 1. 通过 VSCode Chat UI 快速配置基础项

首先，在 VSCode Chat UI 中，点击配置模型：

![点击配置模型的按钮](/img/posts/20260606-b1.png "点击配置模型的按钮")

接下来选择添加 Custom Endpoint 类型的模型：

![添加 Custom Endpoint 类型的模型](/img/posts/20260606-b2.png "添加 Custom Endpoint 类型的模型")

弹出的 Group Name 可任填，这里我填写 `DeepSeek` 表明是 DeepSeek 第一方 API 的组别：

![填写 Group Name（可任填）](/img/posts/20260606-b3.png "填写 Group Name（可任填）")

然后填写自己的 API Key，VSCode 会用自己的 Key 管理器管理，使得在后续的 JSON 配置中，Key 不会直接外漏：

![填写自己的 API Key](/img/posts/20260606-b4.png "填写自己的 API Key")

目前 DeepSeek API 官方只支持 Chat Completions 形式的 OpenAI 接口，这里选择第一项：

![选择合适的接口形式](/img/posts/20260606-b5.png "选择合适的接口形式")

之后 VSCode 会弹出 JSON 编辑页面，进行详细的各个其他选项的编辑。

## Step 2. 在 JSON 配置中进行详细配置

在弹出的 JSON 编辑页面编辑：

![模型 JSON 编辑页面](/img/posts/20260606-b6.png "模型 JSON 编辑页面")

参考的 JSON 配置如下：

```json
[
  {
    "name": "DeepSeek",
    "vendor": "customendpoint",
    "apiKey": "${input:chat.lm.secret.这里每个人的值都不一样，保留 VSCode 默认填写的值}",
    "apiType": "chat-completions",
    "models": [
      {
        "id": "deepseek-v4-pro",
        "name": "DeepSeek V4 Pro",
        "url": "https://api.deepseek.com",
        "toolCalling": true,
        "vision": false,
        "thinking": true,
        "supportsReasoningEffort": ["high", "max"],
        "maxInputTokens": 655360,
        "maxOutputTokens": 393216
      },
      {
        "id": "deepseek-v4-flash",
        "name": "DeepSeek V4 Flash",
        "url": "https://api.deepseek.com",
        "toolCalling": true,
        "vision": false,
        "thinking": true,
        "supportsReasoningEffort": ["high", "max"],
        "maxInputTokens": 655360,
        "maxOutputTokens": 393216
      }
    ]
  }
]
```

**一些关键配置的解释**：
- `id`：即模型的 id 识别名，可从官方文档查阅得到
- `name`：此模型 id 对用户显示的名字，可自定义任意字符串
- `url`：第三方模型的官方 API 地址
- `toolCalling`：模型是否支持调用工具，现在的新模型基本都支持
- `vision`：模型是否支持图片模态的输入，DeepSeek V4 目前是单模态，所以此项填 false
- `thinking`：模型是否支持思维链 COT，现在的新模型基本都支持
- `supportsReasoningEffort`：模型支持的思维力度级别选项
- `maxOutputTokens`：模型最大的输出 Tokens 数量，DeepSeek V4 是 384K，且 DeepSeek 的 K 使用的是 1024 单位，所以 `384 X 1024 = 393216`
- `maxInputTokens`：模型最大的输入 Tokens 数量，DeepSeek V4 的最大上下文是 1M，综上，值是 `(1024 - 384) X 1024 = 655360`

## Step 3. 开始使用

接下来，重启 VSCode，在 Chat 页面就可以选用 DeepSeek V4 模型了：

![VSCode Copilot Chat 使用 DeepSeek V4](/img/posts/20260606-b7.png "VSCode Copilot Chat 使用 DeepSeek V4")

## Future

若将来，DeepSeek 推出了新的模型、支持了多模态输入、改变了上下文窗口大小，均可以在上面的 JSON 编辑页面进行控制：

![VSCode Copilot Chat 的 JSON 模型编辑入口](/img/posts/20260606-b8.png "VSCode Copilot Chat 的 JSON 模型编辑入口")

## References

- [DeepSeek API Doc - Models & Pricing](https://api-docs.deepseek.com/quick_start/pricing)
- [DeepSeek API Doc - Create Chat Completion](https://api-docs.deepseek.com/api/create-chat-completion)
- [VSCode Doc - AI language models in VS Code](https://code.visualstudio.com/docs/agent-customization/language-models#_bring-your-own-language-model-key)
- [VSCode Doc - Language models](https://code.visualstudio.com/docs/agents/concepts/language-models#_bring-your-own-language-model-key)
- [VSCode Doc - AI language models in VS Code](https://code.visualstudio.com/docs/agent-customization/language-models#_bring-your-own-language-model-key)

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
