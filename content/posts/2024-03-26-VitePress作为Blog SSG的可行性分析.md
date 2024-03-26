---
title: "VitePress 作为我的个人博客 SSG 工具的可行性分析"
date: 2024-03-26T19:04:01+08:00
subtitle: "分析一番之后，暂时不迁移博客生成引擎了"  # can be deleted
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: false
tags: ["博客"]
categories: ["博客"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20240326-tech-blog-ssg-vitepress"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

前几天，VitePress 的[正式版 v1.0 发布了](https://blog.vuejs.org/posts/vitepress-1.0)。*而一直很喜欢 VitePress 技术文档风格的我也产生了将 Blog 的 SSG 由 Hugo 迁移到 VitePress 的想法（冲动）。*

VitePress 的默认主题可以说是我所阅读过的技术文档中最为好看的了（当然，按照我的喜好，在默认主题的基础上稍稍自定义一下滚动条便达到我心中的完美了），而且在一些个人 README 渲染的小项目上，VitePress 的 RC 版本很出色的完成了我的任务。更让我意想不到的是，很多第三方库也在逐渐采用 VitePress 作为文档渲染器，例如大名鼎鼎的 [Rollup](https://rollupjs.org/) 和 [D3](https://d3js.org/)。

但在一番调研之后，最终我还是放弃了冲动的想法😂，理由如下：
1. 目前我所使用的 SSG 工具和 Blog 模板是我经过了非常长时间的对比挑选和测试选定下来的，如果要迁移到 VitePress，以我的需求则要亲自从头开始写一个全新的主题样式来复刻我当前使用的模板
2. 相对于专门的 SSG 工具 Hugo 而言，VitePress 的设计更为轻量化，可以说它就是专门为轻量化生成技术文档而生的，所以带来的一个问题便是原有的 Hugo 模板中的大量功能，全部需要在 VitePress 中重新造轮子
3. 同上，原有 Hugo 模板的功能很多，新写一套主题所耗费的时间和精力成本太高了
4. 目前而言我用着 Hugo 没什么大问题，我更期望把时间专注于内容之上和其他的兴趣之上
5. 由 Hugo 迁移到 VitePress，可能造成 GitHub 生成博客的 CI/CD 时间变长（Node.js From Install to Build V.S. Hugo Binary Download and Direct Build）

综上，除非日后我需要用这个新的自造 VitePress 主题当作某些敲门砖，否则我应该是不会再整这么一出大的了😂……

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
