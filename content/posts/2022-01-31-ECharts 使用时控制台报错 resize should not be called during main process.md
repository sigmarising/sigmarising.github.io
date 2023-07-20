---
title: "ECharts 使用时控制台报错 `resize` should not be called during main process"
date: 2022-01-31T09:49:00+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["前端", "CSDN备份"]
categories: ["前端"]
series: ["tech"]
slug: "20220131-tech-echarts-q1"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

此情况可能会出现在 ECharts 与 Vue 3 结合使用的场景下，尤其是将 ECharts 实例使用 ref 包装之后。

本文节选自本人另一篇博文 [Vue 3 开发中的 ECharts 5 使用](/posts/20220131-tech-vue3-echarts5/)

## 1. 问题分析

一般情况下，对 ECharts 图表封装并不需要将 echarts 对象暴露到渲染上下文中。如果确实有意将 echarts 对象声明为响应式，请使用 `shallowRef` 而非 `ref`：

```ts
// GOOD
const chart = shallowRef<echarts.ECharts>(); 

// BAD
const chart = ref<echarts.ECharts>();
```

如果不使用 `shallowRef`，可能会导致命令行报错  \`resize\` should not be called during main process；**事实上，任何第三方库创造的实例都应当使用 `shallowRef` 而非 `ref` 进行响应式处理。**


## 2. 参考链接

* [Github echarts issue - resize error](https://github.com/apache/echarts/issues/15253#issuecomment-870635766)
* [Vue V3 Doc - ref](https://v3.cn.vuejs.org/api/refs-api.html#ref)
* [Vue V3 Doc - shallowRef](https://v3.cn.vuejs.org/api/refs-api.html#shallowref)


---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
