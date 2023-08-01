---
title: "关于 Markdown 的 images 语法"
date: 2023-03-22T17:59:42+08:00
subtitle: "Markdown images 的 alt 和 title 是可以分别设置的"  # can be deleted
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: false
tags: ["开发"]
categories: ["开发"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20230322-markdown-images-syntax"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

我们知道使用 Markdown 引用链接的语法如下：
```md
[链接的文字](/path/to/the/link)
```

而引用图片的语法和引用链接非常相似：
```md
![图片](/path/to/the/img)
```

不过，引用图片的完整语法为：
```md
![img's alt](/path/to/the/img "img's title")
```
其中最后的 `"img's title"` 是可选的。

**实际上，很多的 Markdown 处理引擎处理图片的方式并不太一样，例如关于图注的生成，有些引擎会视 alt 作为题注，而有些则使用 title 作为题注。**

典型的一个例子，静态站点生成器 Hugo 视 alt 和 title 为不同的变量，而很多 Hugo 模板则使用 title 作为题注进行渲染。若你使用的模板恰好为这样的类型，那么在书写 Markdown 时不额外添加 title 信息，Hugo 并不会为你生成图片的题注。

参考链接：
* [Markdown Guide Basic Syntax - images](https://www.markdownguide.org/basic-syntax/#images-1)
* [Caption images with markdown render hooks in Hugo](https://sebastiandedeyne.com/captioned-images-with-markdown-render-hooks-in-hugo/)

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
