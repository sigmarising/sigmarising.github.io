---
title: "嵌套 v-model 解决 Unexpected mutation of “XXX“ prop `vue/no-mutating-props`"
date: 2021-10-11T16:50:35+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["前端", "CSDN备份"]
categories: ["前端"]
series: ["tech"]
slug: "20211011-tech-v-model"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

我们在进行 Vue 开发的时候，经常会遇到一些对可使用 `v-model` 的组件包装嵌套的需求，但若使用不慎，就会引发 `vue/no-mutating-props` 的问题。本文将记录一种在 `v-model` 嵌套时的做法，以避免这个问题。

本文使用 Vue 3 语法。

## 1. 问题复现

我们以 [Naive UI 框架库中的 NDrawer 组件](https://www.naiveui.com/zh-CN/os-theme/components/drawer) 为例，这个组件可以通过 `v-model:show=""` 的方式控制抽屉的开关：
![官方示例用法](/img/posts/55e1b81ebd8e44ec9b138899a8c7bc9f.png "官方示例用法")
不过我们需要对这个组件进行二次封装来满足更多的需求，我的封装代码如下：
![封装代码](/img/posts/bea5e80c97534a368838fc2ad9871210.png "封装代码")
我们可以发现**报错**如下（需要安装 ESLint 插件，VSCode 才会显示行内错误）：
![报错信息](/img/posts/1be9a58eaf8e4b37b38adc5870c9b33e.png "报错信息")
即，**我们对 prop 的内容进行了修改，违反了单向数据流原则。**

> 参考链接：
> * [Vue Doc 单向数据流原则](https://v3.cn.vuejs.org/guide/component-props.html#%E5%8D%95%E5%90%91%E6%95%B0%E6%8D%AE%E6%B5%81)
> * [ESLINT Plugin Vue: vue/no-mutating-props](https://eslint.vuejs.org/rules/no-mutating-props.html)

## 2. 问题分析
实际上，`v-model` 仅仅是一个语法糖而已，它的原理是：父组件通过 props 传入变量，子组件通过事件把更新后的变量值 emit 出来，再由父组件进行事件处理。

所以实质上，在子组件内，我们**并不可以直接将 prop 的变量应用于子组件深层次组件的 v-model 上（因为 v-model 会隐含的对 prop 值更新）**，故上图的用法是肯定会触发错误的。

> 参考链接：
> * [Vue Doc - 表单输入绑定](https://v3.cn.vuejs.org/guide/forms.html)
> * [Vue Doc - 在组件上使用 v-model](https://v3.cn.vuejs.org/guide/component-basics.html#%E5%9C%A8%E7%BB%84%E4%BB%B6%E4%B8%8A%E4%BD%BF%E7%94%A8-v-model)

## 3. 问题解决

解决的方法也非常简单，以上图为例，我们仅需要将 `openDrawer` 通过中间变量进行 v-model 化，再把中间变量传入子组件深层次组件的 v-model 即可：
![props 进行 v-model 化](/img/posts/7240cd9e4e764532b24b14e45fb0422d.png "props 进行 v-model 化")
![中间变量计算属性](/img/posts/1beb00c315704aee98cf7ec4aa2b5c0d.png "中间变量计算属性")
我们利用一个计算属性 `showDrawer` 来实现 v-model，将其 `getter` 设置为从 prop 的 `openDrawer` 初始化，而 `setter` 的内容就是将新的值 emit 出去。

这样，`showDrawer` 这个中间变量（计算属性）便可以应用到 `NDrawer` 的 v-model 上了。

子组件在父组件中的调用代码如下：
![调用代码](/img/posts/cb929981976046779ac21a429b640a4c.png "调用代码")

> 参考链接：
> * [Vue Doc - 在组件上使用 v-model](https://v3.cn.vuejs.org/guide/component-basics.html#%E5%9C%A8%E7%BB%84%E4%BB%B6%E4%B8%8A%E4%BD%BF%E7%94%A8-v-model)
> * [Vue Doc - v-model 参数](https://v3.cn.vuejs.org/guide/component-custom-events.html#v-model-%E5%8F%82%E6%95%B0)


---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
