---
title: "更换 npm 镜像源为 taobao"
date: 2019-01-21T21:48:57+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["前端", "CSDN备份"]
categories: ["前端"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20190121-change-npm-source"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

由于 npm 的默认镜像源在国外，下载速度过慢，影响开发效率，本文介绍替换其为淘宝镜像源的方法。

## 1. 持久替换镜像源

在命令行输入命令：
```shell
npm config set registry https://registry.npm.taobao.org
```

可以持久使用淘宝镜像源。

> 虽然淘宝 NPM 官网推荐用 cnpm 代替 npm，但很多的 cli 工具，内部还是使用 npm 操作的。
>
> 所以，推荐更换 npm 的镜像源，而不是安装 cnpm 来替代。

## 2. 查看 NPM 源地址

可键入如下命令，检查目前使用的 npm 镜像地址：

```shell
npm config get registry
```

## 相关链接

[淘宝 NPM 镜像](https://npm.taobao.org/)


---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
