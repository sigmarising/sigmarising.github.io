---
title: "Claude Code 真的对非 Claude 模型缓存不友好吗？"
date: 2026-04-27T22:10:07+08:00
subtitle: "从 DeepSeekV4 接入 Claude Code，延伸到 AI 时代的模因污染"  # can be deleted
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["AI", "开发"]
categories: ["AI"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20260427-tech-claude-code-3rd-llm-prompt-cache-friendly-or-not"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

## 1. Introduction

总能在互联网上看到一些论调，例如：“Claude Code 对于非 Anthropic 的 LLM 缓存策略并不友好”。**但在这个 AI Slop 营销文满天飞的“模因污染”加速时代，此论点的真实性和可验证性实在让我不解和怀疑**。

正巧赶上前一阵子 Claude Code CLI 误传 Source Map 而泄露了源码，那正好可以从源码角度分析一波到底是否如此。

## 2. Prompt Cache and KV Cache

由于 LLM 普遍采用 Transformer Decoder 的 Shift Right 自回归机制，使得 LLM 在预测下一个 Token 时，所有前序 Tokens 的 KV 矩阵会参与生成新 Token 的 QKV。而**由于自回归特性，所有上下文窗口内前序 Token 的 KV 值是不变的**，这便有了缓存 KV 到 Cache 以加速下一个 Token QKV 计算的实践。

Prompt Cache 就是由此而来，在不同的 Prompt 请求**前缀匹配时，KV 值可以直接复用，而非从头开始重新计算一遍**。

在一个对话 Session 内，用户输入的 Prompt 和 LLM 返回的结果，会以一个历史对话的形式逐渐增长。这样在新的用户输入 or LLM 生成时，历史对话的所有前序 Token 可以被写入 Prompt Cache 中，以加速后续 Token 的计算速度（而非从头开始对历史对话全部计算一遍）。

在 [DeepSeek API Docs - 上下文硬盘缓存](https://api-docs.deepseek.com/zh-cn/guides/kv_cache) 中有较为详细的例子，解释（DeepSeek API 方）Prompt Cache 的运行机制。

当然了，不同的 API 提供方，Prompt Cache 的控制有很大区别。

## 3. Anthropic API

Claude Code 底层也是调用 Anthropic API 发起的请求，这一点和开发者自行购买 API 服务然后写代码调用没有任何区别。

Anthropic API 的 Prompt Cache 有两种控制办法，自动控制和手动控制。自动控制会在每轮 User Input 的位置放置 Cache Point，而手动控制可以更加灵活一些，Cache Point 可由开发者自行放置在 Message 的合适位置（最多四个 Cache Points）。作为参考，Claude Code 会在 User Input 的位置自动放置 Cache Point。

缓存写入时，Anthropic API **会将 Prompt 从最开始到 Cache Point 位置的 Token 序列计算一个 Hash 值作为 KV-Cache Key**，而这段 Token 的 KV 则作为 KV-Cache Value 存储。也就是 **Prompt Cache Write 只会发生在 Cache Point 的位置**。

**当新的一轮请求来临时：**
- 服务端会基于最新请求的 Cache Point 计算前缀的 Hash，以去匹配 KV-Cache Key
- 若没有匹配到，服务端会基于请求的 Cache Point 向前回退 20 Block，并计算每次的回退的前缀 Hash 来匹配 KV-Cache Key
- 因为 Cache Point 可能有多个，每个 Cache Point 都会进行一遍上述逻辑，直到匹配完成
- **若以上全部匹配失败，则 Prompt Cache 命中失败**
- *额外注意：Anthropic API 视 5 分钟缓存和 60 分钟缓存为两种不同类型的缓存，即便前缀匹配，两者之间也不可互相命中*

参考资料：
- [Claude API Docs - Prompt caching](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)
- [Claude API Docs - Building with extended thinking](https://platform.claude.com/docs/en/build-with-claude/extended-thinking#thinking-block-preservation-in-claude-opus-4-5-and-later)

是不是听着很复杂？尤其是非技术研发人员可能更是摸不着头脑。但我们也可以从参考资料和 KV Cache 原理得出结论：**强如 A 社，也必须依赖 Token 前缀匹配来完成 Prompt Cache，任何中间的动态信息都会 Break Prompt Cache！原理面前众生平等。**

## 4. Claude Code - Cache Edit / Cache Reference

在 Claude Code 的源码中，除了 Cache Control 外，还有两个 A 社不公开的 Cache Edit 和 Cache Reference 的 BETA 字段。总结来说，它们是用来标记上下文中哪些太过久远的 Tool Call 和 Tool Result 可以被服务端计算 Attention 时移除掉的（由于服务端逻辑未知，**此为推测，相当于服务端做了一次微小的上下文压缩处理**）。**但是在会话中，对应的 Tool Call 和 Tool Result 并不会被移除**（因为要在客户端渲染 UI），而且**发送 Anthropic API 时，完整内容照发不误**。

虽然 Claude Code 的 System Prompt 和 Tool Define 的确是动态组装的，但是一旦进入同一个 Session 内，这些字符串就全部被计算敲定了。Claude Code 在诸多方面都做了一个努力：**保证同一个 Session 内 Prompt 上下文的连续性，不通过“透传”等手段附加任何可能破坏 Prompt Cache 的动态信息**。

## 5. DeepSeek API

相比之下，DeepSeek 的 API 就简单太多了，它的 Prompt Cache 完全无需用户/开发者操作，一切在服务端自动完成。

Prompt Cache 写入逻辑：
- 服务端自动在 User Input 和 LLM Output 位置写 Prompt Cache
- 若多次请求只有前缀匹配（例如动态信息“透传”），则服务端会缓存公共前缀写 Prompt Cache
- 服务端自动在长输入/输出时，以固定 Token 间隔写 Prompt Cache

缓存命中时，DeepSeek 似乎没有 Anthropic 那么复杂的基于 Cache Point 回退 20 Block 计算 Hash 的超复杂操作；DeepSeek API 只要前缀匹配，即可命中对应的 Prompt Cache。

参考资料：
- [DeepSeek API Docs - 上下文硬盘缓存](https://api-docs.deepseek.com/zh-cn/guides/kv_cache)
- [DeepSeek API Docs - Anthropic API](https://api-docs.deepseek.com/zh-cn/guides/anthropic_api)

但值得注意的是：
- DeepSeek API 写/读缓存的机制描述的过于通俗，而且官方也说了“尽力而为”，所以实际上难以判断真实逻辑（从而确定实际是否写了或者读了 Prompt Cache）
- DeepSeek 并没说明缓存的详细过期时间策略
- DeepSeek API 对大量 Anthropic API 的不支持字段做了忽略处理，而实际上 Claude Code 在确保同一 Session 内 Prompt 上下文连续性做了充足努力，所以**可以推测出，实际上 DeepSeek 接入 Claude Code 的 Prompt Cache 友好度应该会非常理想**

## 6. 缓存价格

Anthropic 的写缓存是有成本的，它的价格比正常输入都要贵。所以可想而知，Claude Code 必然需要对上下文管理的更加 Prompt Cache 友好：

![Anthropic API 的模型价格，缓存写入价格十分感人](/img/posts/20260427-c1.jpg "Anthropic API 的模型价格，缓存写入价格十分感人")

DeepSeek API 的写缓存不会计费（相比之下，真的十分良心）。

## 7. x-anthropic-billing-header

Claude Code 接入第一方 Claude API 时，会在 System Prompt 的首条消息注入 `text=x-anthropic-billing-header: cc_version=2.1.133.4b3; cc_entrypoint=cli; cch=2b618;`（其中 `cch` 是动态变化的）用于计费目的。由于 Anthropic 第一方 API 会对此消息做 filter 处理，所以并不会破坏第一方 Claude 模型使用的 Prompt Cache。

事实上，此动态注入[在 Claude Code 的文档中也被清晰标明](https://code.claude.com/docs/en/env-vars#:~:text=CLAUDE_CODE_ATTRIBUTION_HEADER)了，大部分的国产第一方 LLM API（包括 DeepSeek API）已经对此首轮 System Prompt 做了和 Anthropic 一致的 filter 处理。对于即便没有主动 filter 此消息的 LLM API，也可以通过配置 `"CLAUDE_CODE_ATTRIBUTION_HEADER":"0"` 来轻松禁用 `x-anthropic-billing-header` 的注入。

## 8. 相关话题模因污染

强如 `Opus 4.7[1m]-MAX`，在分析 Claude Code 源码、实际工程开发时，都会产生不少的幻觉，更何况满天飞的 AI Slop 营销号呢？

下面就是一个典型的模因污染案例（此人可能想表达本文上一章节中的内容，但在未经任何验证的情况下被 AI 幻觉完全带偏了）：

![一位某书博主的自信图文，存在多处事实错误](/img/posts/20260427-c2.jpg "一位某书博主的自信图文，存在多处事实错误")

而实际上，各大用户广泛的使用反馈是，DeepSeekV4 in Claude Code 的缓存命中率极其理想！而我使用的体感也完全是如此：

![个人使用 DeepSeekV4 in Claude Code，缓存命中效果非常理想](/img/posts/20260427-c3.jpg "个人使用 DeepSeekV4 in Claude Code，缓存命中效果非常理想")

**虽然现在是 AI 加速时代，但多加一层 Verified（无论是人还是 AI 来做），都是很有必要的**。\
*扩展阅读：[AI 时代正在加速模因污染 - Claude Code CLI 源码泄露之外](/posts/20260402-tech-ai-speed-up-meme-pollution)*


---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
