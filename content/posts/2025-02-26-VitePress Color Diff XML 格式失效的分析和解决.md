---
title: "VitePress Color Diff XML 格式失效的分析和解决"
date: 2025-02-26T20:31:11+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["前端"]
categories: ["前端"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20250226-tech-vitepress-color-diff-xml"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

## 1. 问题描述

VitePress 的 Markdown 扩展可以通过在行尾注释中添加 `[!code ++]` 和 `[!code --]` 来达到 Color Diff 的效果（参考此[官方文档](https://vitepress.dev/guide/markdown#colored-diffs-in-code-blocks)）。然而，在 XML 格式的代码中，此功能出现了一些问题。

问题代码如下：

    ```xml
    <note>
        <to>Tove</to> <!-- [!code ++] -->
        <from>Jani</from> <!-- [!code --] -->
        <heading>Reminder</heading> <!-- [!code ++] -->
        <body>Don't forget me this weekend!</body>
    </note>
    ```

渲染效果如下：

![XML Color Diff 失效](/img/posts/20250226-v1.png "XML Color Diff 失效")

## 2. 分析

根据[此文档](https://vitepress.dev/reference/site-config#markdown)可知，VitePress 使用 [Shiki](https://github.com/shikijs/shiki) 作为语法高亮的工具，并且 `[!code ++]` 和 `[!code --]` 的用法也是继承自 Shiki 的官方 `transformerNotationDiff`（参考此[链接](https://shiki.style/packages/transformers#transformernotationdiff)）。

而通过参考 [StackOverflow: How do I comment out a block of tags in XML?](https://stackoverflow.com/questions/2757396/how-do-i-comment-out-a-block-of-tags-in-xml) 可知，XML 的 `<!-- Comment -->` 注释中，若 Comment 含有 `--` 字符，Comment 会异常终止。

综上，这显然是 Shiki 库在处理 XML Code 时候的一个 BUG，并且社区中已经有开发者发现并提出了相关 [issue](https://github.com/shikijs/shiki/issues/928)。

## 3. 解决办法

来自 VitePress 的[相关 issue 讨论中](https://github.com/vuejs/vitepress/issues/4555)，VitePress 维护者提供了一个临时的解决方案，即通过配置自定义的 codeTransformers 来规避此问题。

即在 VitePress 的配置文件中（需自行安装 pkg `@shikijs/transformers`）：
```ts
import { defineConfig } from "vitepress";
import { transformerNotationMap } from "@shikijs/transformers"; // Add this line

export default defineConfig({
  title: "My Awesome Project",
  description: "A VitePress Site",

  // Add the following code
  markdown: {
    codeTransformers: [
      transformerNotationMap({
        classMap: { add: "diff add", del: "diff remove" },
        classActivePre: "has-diff",
        matchAlgorithm: "v3",
      }) as any,
    ],
  },

});
```

此时，就可以使用如下的 Color Diff 了：

    ```xml
    <note>
        <to>Tove</to> <!-- [!code add] -->
        <from>Jani</from> <!-- [!code del] -->
        <heading>Reminder</heading> <!-- [!code add] -->
        <body>Don't forget me this weekend!</body>
    </note>
    ```


渲染效果如下：

![XML Color Diff Fix](/img/posts/20250226-v2.png "XML Color Diff Fix")

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
