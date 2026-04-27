---
title: "DeepSeek V4 接入 Claude Code 简易指南"
date: 2026-04-26T15:52:27+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["AI", "开发"]
categories: ["AI"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20260426-tech-deepseek-v4-claude-code"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

## Introduction

2026 年 4 月 24 日，DeepSeek v4 版本发布。官方提供了旗舰模型 DeepSeek v4 Pro 和小模型 DeepSeek v4 Flash 两个版本。在官方文档中，也提供了[简易的 Claude Code 接入指南](https://api-docs.deepseek.com/zh-cn/guides/coding_agents)，但是这个指南**个人认为并非最佳配置形式**，所以在后文给出我的更加合理的配置方案。

## 1. DeepSeek 给出的配置以及此配置的相关问题

### 1.1. DeepSeek 配置方案

DeepSeek 给出的 Claude Code 配置方法与通过 Amazon Bedrock 配置 Claude 模型的方法有些相似，*不过 DeepSeek 只给出了通过环境变量配置的方法*。而且 DeepSeek 也分别在 4 月 24 日 和 4 月 25 日更新了两版本配置内容，这两版内容均有相应的槽点。

[4 月 24 日版配置](https://web.archive.org/web/20260424094820/https://api-docs.deepseek.com/zh-cn/guides/coding_agents)：
```bash
export ANTHROPIC_BASE_URL=https://api.deepseek.com/anthropic
export ANTHROPIC_AUTH_TOKEN=${DEEPSEEK_API_KEY}
export ANTHROPIC_MODEL=deepseek-v4-pro[1m]
export ANTHROPIC_DEFAULT_OPUS_MODEL=deepseek-v4-pro
export ANTHROPIC_DEFAULT_SONNET_MODEL=deepseek-v4-pro
export ANTHROPIC_DEFAULT_HAIKU_MODEL=deepseek-v4-flash
export CLAUDE_CODE_SUBAGENT_MODEL=deepseek-v4-pro
export CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC=1
export CLAUDE_CODE_DISABLE_NONSTREAMING_FALLBACK=1
export CLAUDE_CODE_EFFORT_LEVEL=max
```

[4 月 25 日版配置](https://api-docs.deepseek.com/zh-cn/guides/coding_agents)：
```bash
export ANTHROPIC_BASE_URL=https://api.deepseek.com/anthropic
export ANTHROPIC_AUTH_TOKEN=<你的 DeepSeek API Key>
export ANTHROPIC_MODEL=deepseek-v4-pro[1m]
export ANTHROPIC_DEFAULT_OPUS_MODEL=deepseek-v4-pro[1m]
export ANTHROPIC_DEFAULT_SONNET_MODEL=deepseek-v4-pro[1m]
export ANTHROPIC_DEFAULT_HAIKU_MODEL=deepseek-v4-flash
export CLAUDE_CODE_SUBAGENT_MODEL=deepseek-v4-flash
export CLAUDE_CODE_EFFORT_LEVEL=max
```

### 1.2. 相关问题

#### 1.2.1. 配置文件位置

第一个问题就是，如今更为推荐的 Claude Code 配置方法，是通过 `~/.claude/settings.json` 进行配置（Windows 的 `～/` 对应当前用户文件夹）。这个配置文件可以被 CLI 和 VSCode Extension 共同消费使用，并且**不会污染系统环境变量**。

#### 1.2.2. DEFAULT MODEL 配置

第二个问题在于，`ANTHROPIC_MODEL` 这个变量**在配置了 DEFAULT OPUS/SONNET/HAIKU 模型之后，是无需配置的**。最新版的 Claude Code 并不会从这个变量去读取当前需要使用的模型，而是通过 `settings.json` 中的 `model` 字段进行偏好记忆控制。在实际使用时，按需切换 OPUS/SONNET/HAIKU 模型即可（我配置公司的 Amazon Bedrock Claude Model 也是按此方式操作的，没有任何的使用问题）。

在实践中，将 OPUS/SONNET 映射为 DeepSeek-v4-Pro，而 HAIKU 映射为 DeepSeek-v4-Flash 的做法是正确的（因为这个 Flash 模型实在是很小，甚至比当年的 DeepSeek-R1（标准版，非蒸馏版）还要小的多……也只能将其映射为 HAIKU 的定位了……）

> 参考链接：[Claude Code Docs - Model Configuration #setting-your-model](https://code.claude.com/docs/en/model-config#setting-your-model)

#### 1.2.3. `[1M]` Context 配置

第三个问题，其实官方 4 月 25 的新版配置修正了一部分，即为 DEFAULT OPUS/SONNET 模型添加了 `[1m]` 参数。因为官方配置的方法非常类似于通过 Amazon Bedrock 配置 Claude 模型的方法，所以 `[1m]` 参数是最快速启用 1M 上下文的手段。这样在切换 Opus/Sonnet 模型时，其映射的 DeepSeek v4 Pro 模型可以开启 1M 上下文参数（而非默认的 200K）。

更进一步的，其实 DEFAULT HAIKU 模型的配置也可以添加 `[1m]` 参数。虽然 Claude Haiku 只有 200K 上下文，但是**通过映射后的 DeepSeek v4 Flash 是可以开启 1M 上下文的**（可以通过 `/context` 进行验证）。

> 参考链接：[Claude Code Docs - Model Configuration #pin-models-for-third-party-deployments](https://code.claude.com/docs/en/model-config#pin-models-for-third-party-deployments)

#### 1.2.4. SUBAGENT MODEL 配置 

最后一个问题，**就是 `CLAUDE_CODE_SUBAGENT_MODEL` 不应该被配置**。两个版本 DeepSeek 官方文档非常难绷的将 SubAgent 的模型强制锁定为了 Pro/Flash 二选一，但实际上，Claude Code 大量的 Build-in SubAgent 会根据任务的种类，使用不同的 Haiku/Sonnet/Inherit 模型进行工作负载的高低按需运行（更加具备经济性、省钱）。

在配置了 DEFAULT OPUS/SONNET/HAIKU 模型之后，`CLAUDE_CODE_SUBAGENT_MODEL` 更是无需被配置。

> 参考链接：[Claude Code Docs - Create custom subAgents #built-in-subagents](https://code.claude.com/docs/en/sub-agents#built-in-subagents)

#### 1.2.5. 其他配置

关于思考强度 `CLAUDE_CODE_EFFORT_LEVEL`，按照官方的建议直接配置为 `max` 即可，Claude Code 会自行将 max 映射为 OPUS/SONNET/HAIKU 实际能用的 Effort，而 DeepSeek API 则可以正确将这些 Effort Level 映射为 High/MAX 级别，非常省心。

官方 4 月 25 日版的配置删去了 `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC=1` 和 `CLAUDE_CODE_DISABLE_NONSTREAMING_FALLBACK=1`。其中前者禁用了 Claude Code 的自动更新、反馈、遥测日志等和核心功能无关的功能，建议打开；而后者禁用了在流式传输错误时的非流式传输回退，可开可不开。

> 参考链接:\
> [Claude Code Docs - Model Configuration #adjust-effort-level](https://code.claude.com/docs/en/model-config#adjust-effort-level)\
> [DeepSeek API Docs - Thinking Mode](https://api-docs.deepseek.com/zh-cn/guides/thinking_mode#%E6%80%9D%E8%80%83%E6%A8%A1%E5%BC%8F%E5%BC%80%E5%85%B3%E4%B8%8E%E6%80%9D%E8%80%83%E5%BC%BA%E5%BA%A6%E6%8E%A7%E5%88%B6)

## 2. 更合理的配置方法

### 2.1. 配置位置

Posix（Linux/Mac）推荐在 `~/.claude/settings.json` 中配置。\
Windows 则推荐在 `C:\Users\<你的实际用户名>\.claude\settings.json` 中配置。

**以上文件若不存在，自行创建即可**。

### 2.2. 配置内容

必选配置：
```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.deepseek.com/anthropic",
    "ANTHROPIC_AUTH_TOKEN": "<你的 DeepSeek API Key>",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "deepseek-v4-pro[1m]",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "deepseek-v4-pro[1m]",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "deepseek-v4-flash[1m]",
    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": "1",
    "CLAUDE_CODE_EFFORT_LEVEL": "max"
  },
  "model": "opus"
}
```

可选配置：
```jsonc
{
  "env": {
    # 启用实验性质的 Agent 团队功能
    # - 此功能稳定性一般，视自己的需求而定
    # - 此配置可开可不开
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1",

    # 禁用 Claude Code 的一些 Beta Request Headers
    # - 由于 DeepSeek API 会自动忽略相关 Headers
    # - 此配置可开可不开
    "CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS": "1",

    # 禁用流式传输失败时，以非流式传输的形式回退
    # - 此配置可开可不开
    "CLAUDE_CODE_DISABLE_NONSTREAMING_FALLBACK": "1"
  }
}
```

> 参考链接：\
> [Claude Code Docs - Environment Variables](https://code.claude.com/docs/en/env-vars)\
> [DeepSeek API Docs - Anthropic API](https://api-docs.deepseek.com/zh-cn/guides/anthropic_api)

### 2.3. 使用

配置完成后，Claude Code CLI 和 VSCode Extension（需要 disable login，参考[官方文档](https://code.claude.com/docs/en/vs-code#use-third-party-providers)）中即可使用 DeepSeek v4 系列模型了。

由于 Claude Code（CLI 和 VSCode Extension）的 BUG 都不少，建议时不时更新一下 Claude Code 版本。 

## 3. 其他相关

### 3.1. Claude Code 使用非 Claude 模型的 Prompt Cache 友好程度？

详见文章：[Claude Code 真的对非 Claude 模型缓存不友好吗？](/posts/20260427-tech-claude-code-3rd-llm-prompt-cache-friendly-or-not)

### 3.2. CodeX 使用 DeepSeek 作为基模的可能性？

在 2026 年初，OpenAI 更改了 CodeX 的配置选项，使 `model_providers.<id>.wire_api` 的可配置项只能是 `responses` 了。这就导致除非配置反代，否则无法轻松的配置其他 LLM Service 作为 CodeX 的基模。

> 参考链接：[Configure OpenAI Codex CLI with DeepSeek Support](https://gist.github.com/antenore/c529e055e45559579b08b4961b517f8c)

但也无妨，毕竟 CodeX 那思路惊奇的 FreeForm Files Edit 也不是 GPT 以外的 LLM 友好的。\
虽然国人天天喷 A 社，但在工具开放性这一点上，它还是比 O 社强不少的……

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
