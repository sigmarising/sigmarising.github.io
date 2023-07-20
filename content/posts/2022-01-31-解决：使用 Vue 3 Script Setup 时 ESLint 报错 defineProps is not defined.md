---
title: "解决：使用 Vue 3 Script Setup 时 ESLint 报错 ‘defineProps‘ is not defined"
date: 2022-01-31T10:37:17+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["前端", "CSDN备份"]
categories: ["前端"]
series: ["tech"]
slug: "20220131-tech-vue3-eslint"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

Vue 3 的 Script Setup 语法引入了 `defineProps`、`defineEmits`、`defineExpose`、`withDefaults` 的编译器宏。然而某些情况下，ESLint 会报错以上编译器宏函数未定义。

本文将介绍两种解决方案来解决这个问题（假定你的项目使用 Vue-Cli 进行初始化）。

## Step 1. 检查 `eslint-plugin-vue` 的版本

```shell
npm list eslint-plugin-vue
```

若版本在 v8.0.0 以上，跳转到 Step 2，否则直接到 Step 3 的内容。

## Step 2. 版本为 v8.0.0+

打开 `.eslintrc.js` 文件并修改如下：
```js
  env: {
    node: true,
    // The Follow config only works with eslint-plugin-vue v8.0.0+
    "vue/setup-compiler-macros": true,
  },
```

## Step 3. 版本为 v8.0.0 以下

打开 `.eslintrc.js` 文件并修改如下：
```js
  // The Follow configs works with eslint-plugin-vue v7.x.x
  globals: {
    defineProps: "readonly",
    defineEmits: "readonly",
    defineExpose: "readonly",
    withDefaults: "readonly",
  },
```

> 若你的 `eslint-plugin-vue` 版本在 v8 以下，不建议贸然升级版本（尤其是在使用了大量 ts 依赖时）。

## 参考链接

* [vue3报错：‘defineProps‘ is not defined，‘defineExpose‘ is not defined](https://blog.csdn.net/zhanye88/article/details/121644706)
* [Compiler macros such as defineProps and defineEmits generate no-undef warnings](https://eslint.vuejs.org/user-guide/#compiler-macros-such-as-defineprops-and-defineemits-generate-no-undef-warnings)
* [eslint compile time error - Environment key "vue/setup-compiler-macros" is unknown](https://github.com/vuejs/eslint-plugin-vue/issues/1695)
* [What is causing Error: .eslintrc.js: Environment key "vue/setup-compiler-macros" is unknown](https://stackoverflow.com/questions/69796772/what-is-causing-error-eslintrc-js-environment-key-vue-setup-compiler-macros)


---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
