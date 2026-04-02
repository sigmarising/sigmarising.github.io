---
title: "AI 时代正在加速模因污染"
date: 2026-04-02T20:45:44+08:00
subtitle: "Claude Code CLI 源码泄露之外"  # can be deleted
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["AI", "开发"]
categories: ["AI"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20260402-tech-ai-speed-up-meme-pollution"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

## Prologue

> 背景：Claude Code CLI，是目前各界 IT 研发人员认可度最高的 Coding Agent/Harness，地位几乎相当于此领域目前的 State of the Art

3 月 30 日的 Claude Code CLI 源码泄露，无疑是各大 Agent/Harness 研发人员的狂欢。

但在狂欢之外，一个一直未被大多数人注意的小细节，引发了我的深思——**“AI 时代，正在加速模因污染”**。

## SourceMap —— 风暴之源

作为一名多年的前端研发人员，SourceMap 不可谓不熟悉，它能轻松的还原混淆后的生产代码，便于开发调试。在 3 月 31 日晚，我在前司的前端生活微信群里得知了 Claude Code 误传 SourceMap 到 NPM 源的消息，随后我光速加入为吃瓜一员。

在快速确认了 NPM 上 Claude Code 的 2.1.88 版本所含的 `.map` 文件还未被撤回后，我光速通过 `npm pack @anthropic-ai/claude-code@2.1.88` 下载了第一手代码，随后很简单的通过 `npx -y reverse-sourcemap --output-dir ./src ./cli.js.map` 反编译出了源代码。

吃瓜不嫌事大，随后我便开始在知乎、小红书等各大平台冲浪相关新闻。但在冲浪时，一个**频繁出现的奇怪描述**引起了我的注意：

> ...包里残留了 `.map` 文件，里面有个指向 Anthropic R2 存储桶的下载链接...
> 
> ...附带了一个 Cloudflare R2 存储桶的链接...

看到此类描述后，我的第一反应就是，这和我多年的前端开发经验对不上啊？如果真的是通过 Mapping URL 指向源码，为什么 `.map` 文件内容高达 56.9 MB？**Anthropic 真的会犯在 Cloudflare R2 上存储明文代码的低级错误**？难道是还有一些代码没有反编译出来？

抱着好奇心理，我随手对 `.map` 文件进行了 AI Python 脚本辅助的数据挖掘，结论是 `.map` 中无任何 Secret 和 Cloudflare R2 相关的信息。

![AI 辅助 Python 脚本数据分析结果 —— 纯粹就是源代码，无任何其他信息](/img/posts/20260402-m2.jpg "AI 辅助 Python 脚本数据分析结果 —— 纯粹就是源代码，无任何其他信息")

之后我没有多想，觉得大概率是营销号用 AI 幻觉瞎写的营销文传播罢了，便继续进行了两日忙碌的工作。

## Cloudflare R2 信息由何而来？

今天稍微有空，我溯源到了最初挖掘到 Anthropic 误传了 `.map` 文件的始发现者，**稍读推文后，我恍然大悟，明白了 Cloudflare R2 存储桶的误传信息是由何而来了……**

![始发现者推特推文](/img/posts/20260402-m1.jpeg "始发现者推特推文")

始发现者 Chaofan Shou 只是**将自己反编译过的源代码，随手分享在了 Cloudflare R2 存储桶的链接里，而并非是 `.map` 文件中包含有指向 Cloudflare R2 的源代码文件……**

然后这条推文先是在国外，被 AI 快速信息提取出了幻觉的结论：

> The source map also referenced a ZIP archive hosted on Anthropic's own Cloudflare R2 storage bucket, downloadable by anyone with the URL.
>
> The source map didn't contain the source code directly. It referenced it, pointing to a URL of a .zip file hosted on Anthropic's own Cloudflare R2 storage bucket.

**然后被国内各营销号无差别搬运至国内互联网，期间整条链路上，没有人 / AI对 `.map` 文件做任何的验证。**

```txt
# 推特始发现者附上的原 Cloudflare R2 链接，如今早已 404 了
https://pub-aea8527898604c1bbb12468b1581d95e.r2.dev/src.zip
```

## 模因污染的互联网，没有记忆

才过了两天，SourceMap 含有指向 Cloudflare R2 存储桶中源码的信息已经基本在各大 APP 的推荐算法中看不到了。但我相信，**直到现在（甚至未来），还会有相当多的人相信“源代码是通过 Cloudflare R2 中存储的源码泄露的”这个不实信息**。

说实话，我的前端技术并不算高，我相信一定会有不少人也发现了“SourceMap 映射 Cloudflare R2 中的源码”这个信息是错误的；但更多的人（非前端研发、非研发、非 IT 人士、营销号等）依然会将这已被污染的模因继续“遗传”下去。

AI 时代，LLM 时代，Agent 时代，Harness 时代，一切都在疯狂的加速。**虽然从互联网诞生的那一刻开始，模因污染已然开始，但在 AI 浪潮的无情加速中，模因污染的速度也被进一步的加快了。**


### 相关模因污染 Case 1

Claude Code CLI 源码泄露发酵之初，由于很多人不具备反编译 `.map` 文件的能力，以及无法方便的全局上网搜索到始发现者贴出的链接（或者链接很快被 404 了），一位 Github 的用户，通过第一时间上传了 Claude Code CLI 的反编译源码，并在宣传时间上占据了先机，使得 `github.com/instructkr/claude-code` 光速成为了 Github 历史上最快 50K Star 数的仓库。

![instructkr\/claude-code 的 Star 增长速度](/img/posts/20260402-m3.jpg "instructkr/claude-code 的 Star 增长速度")

但不出我所料，不管是什么原因（Anthropic DCMA 也好，作者夹带私货也好），这位作者很快将仓库改为了 `ultraworkers/claw-code` 并附上了自己删除源码以及重写版本的“小作文”。

我不评价此作者的动机如何，但我相信，如果这位作者一开始开源的不是源码而是自己的重写版本，这个仓库是绝对不可能 Star 涨的这么快的……**原因无它，重写版本有很多很多，但大家一开始都是冲着源码去的**。

![作者的小作文](/img/posts/20260402-m4.jpg "作者的小作文")

更令人哭笑不得的是，还有不少的国内营销号硬吹此作者的“对抗”精神，真的将模因污染诠释的淋漓尽致。

### 相关模因污染 Case 2

![一名主动“认错者”](/img/posts/20260402-m5.jpg "一名主动“认错者”")

一位推特博主，“承认自己误传 `.map` 而被 Anthropic 开除”，但很快又被网友打脸。

![网友光速打脸](/img/posts/20260402-m6.jpg "网友光速打脸")

但在我前司的脉脉同事圈，已经有不少人被此虚假信息蒙蔽，将这污染的模因继承下去了……

## Epilogue

我相信在互联网的洪流里，这些真相并不会被多少人看到和了解，而是被快速冲散和淹没在信息的巨浪之中。

大多数人都蒙在鼓里，都在加速，都在“赢”，都在“键盘侠”一样抬杠和营销自己。**没人在乎事件周边细节的真相和事实，那么下一步，事件本身的真相和事实，也快了……**

**AI 时代，LLM 时代，Agent 时代，Harness 时代，模因污染的速度也被进一步的加快了。**

不禁感叹，真是应了小岛秀夫二十五年前在《潜龙谍影2：自由之子》中的预言啊……

![这个世界正在被“真相”淹没](/img/posts/20260402-m7.jpeg "这个世界正在被“真相”淹没")

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
