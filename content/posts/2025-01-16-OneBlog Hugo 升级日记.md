---
title: "OneBlog Hugo 升级日记"
date: 2025-01-16T17:53:49+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: false
tags: ["博客", "开发"]
categories: ["博客"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20250116-tech-one-blog-hugo-update"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

自从我构建 OneBlog 以来，其 SSG 工具 Hugo 的版本升级共有两次，分别是 [v0.111.3 版本的初始化](https://github.com/sigmarising/sigmarising.github.io/commit/6c50ccf5edcb9b0f8868cd61b51de8aaa22db998)以及[升级到 v0.123.7 版本](https://github.com/sigmarising/sigmarising.github.io/commit/5a0c9ef9a25dd9c104bac272f2a2a62f879435b2)，**时间间隔为一年，升级过程顺利无感**，甚至完全不需要对博客主题模板进行修改。

昨天，也就是距离上一次我对 OneBlog SSG 工具 Hugo 版本升级**仅过了 10 个月**的时间，我在想要升级 Hugo 到最新版的 [v0.140.2](https://github.com/gohugoio/hugo/releases/tag/v0.140.2) 时，却迎来了当头一棒，Hugo 开始撒了欢给我报各种 WARN 和 Deprecate ERROR，大致的内容和下面的文章类似：
- [Help with errors](https://discourse.gohugo.io/t/help-with-errors/51446)
- [Pagination Deprecated Error](https://discourse.gohugo.io/t/pagination-deprecated-error/51571)

在经过了还不算特别痛苦的修复之后，我的 OneBlog 成功跑了起来，但是我发现了一个严重的问题，OneBlog 的搜索功能和所有文章的 Summary 内容表现出了和原先旧版本 Hugo 渲染的十分大的差异，特别是搜索功能几乎完全排版炸掉。在经过了对多个 Hugo release 版本的测试之后，我发现了核心问题所在。

在 Hugo [v0.134.0](https://github.com/gohugoio/hugo/releases/tag/v0.134.0) 及以后版本中，Hugo 的 Summary 引擎彻底进行了重写，其 Behavior 与旧版本完全不一样了。而我所使用的 Blog 主题模板大量依赖于旧特性的 Summary 功能，在切换到新版本 Hugo 后，我的 Blog 渲染生成的 `index.xml`、`index.json`、首页文章 Summary、文章页 `<head>` 标签内 description 全部受到了严重影响。由于 `index.xml`、`index.json` 在新版本 Hugo 输出后会带有 HTML 样式转义（而我只需要纯文本），并且 `<head>` 标签内 description 难以简单控制（**说白了，就是我认为我并不值得花费过多的时间和精力去学习 Hugo 语法，去把博客主题模板升级适配到最新 Hugo 版本，尤其是在 Hugo 的 Breaking Change 越来越快的现状之下**），故 OneBlog 将会锁定 Hugo 版本在 v0.134.0 之前的最新一个 release 版本上（[v0.133.1](https://github.com/gohugoio/hugo/releases/tag/v0.133.1)），毕竟 Hugo 的那些 Breaking Change 我也用不上……

最后小小吐槽一下 Hugo 吧……同样是一年时间，去年相比前年，Hugo 的 Breaking Change 简直数量惊人，尤其是其 Release 说明写的非常随意，也不附有详细的版本升级指南，哪个 API deprecate 了只有运行的时候才告诉你（让我想起了 TensorFlow v1）。虽然客观讲，这是 Hugo 仍然具备开发者生命力的表现，但在 Vite SSG 大流行的趋势下，我实在是不想把过多时间投入在一个不占流行趋势的 SSG 工具上……

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
