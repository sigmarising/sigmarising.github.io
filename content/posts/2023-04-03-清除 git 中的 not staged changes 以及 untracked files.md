---
title: "清除 Git 中的 Not Staged Changes 以及 Untracked Files"
date: 2023-04-03T19:20:25+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["git"]
categories: ["git"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20230403-git-restore-clean"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

有时我们想要清理 git 项目中所有没被 commit 的更改，而这又被 git 分为了 not staged changes 以及 untracked files 两种。本文将介绍两个指令来分别清理对应的文件。

## Clean All Not Staged Changes

假设此时我们处于项目的根目录，此时可以使用 git 命令：
```shell
git checkout -- .
# 等价于
git restore .
```
此命令会清除所有 Not Staged 的文件更改。

## Clean All Untracked Files

假设此时我们处于项目的根目录，此时可以使用 git 命令：
```shell
# 打印将被删除的文件和文件夹
git clean -nfd
# 执行删除
git clean -fd
```

`git clean` 参数说明：
* `-f`: 强制删除文件
* `-fd`: 删除文件和文件夹
* `-n`: 打印将被删除的文件和文件夹

## 参考链接

* [清除 git 中 Untracked files](https://www.jianshu.com/p/e8d4ded3aacf)

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
