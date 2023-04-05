---
title: "npm scripts 中的钩子"
date: 2023-04-05T16:43:05+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["前端"]
categories: ["前端"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20230405-npm-scripts-hooks"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

我们知道，在 `package.json` 的 scripts 字段可以设置很多自定义命令，然后通过 `npm run xxx` 来执行。

但值得注意的是如下的情况：

```json
{
  "scripts": {
    "prebuild": "echo ABC",
    "build": "cross-env NODE_ENV=production webpack",
    "postbuild": "echo 321"
  }
}
```

此时执行 `npm run build`，实际上执行的则是：
```shell
npm run prebuild && npm run build && npm run postbuild
```

npm 脚本有 `pre` 和 `post` 两个钩子，分别在目标命令的前后执行。**这里便存在了一个陷阱，若 `pre` 钩子执行失败，那么后续的指令将不会继续执行。所以在编写 npm scripts 时，请务必留意是否有对应的钩子 scripts 存在。**

## 参考链接

[npm scripts 使用指南](https://www.ruanyifeng.com/blog/2016/10/npm_scripts.html)

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
