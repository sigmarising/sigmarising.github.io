---
title: "Git 命令对比 Checkout Restore Switch"
date: 2023-04-03T19:21:44+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: false
tags: ["git"]
categories: ["git"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20230403-git-restore-switch"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

自 [git version 2.23](https://github.com/git/git/blob/master/Documentation/RelNotes/2.23.0.txt) 开始，git 引入了新命令 `restore` 和 `switch`。由于 `checkout` 这个命令能干太多的事情了，所以这两个新命令主要用于分担“责任过重”的 `checkout` 的语义理解负担。当然，我们依旧可以继续使用 checkout 来完成这些事情。

restore 的例子：
```shell
# 清除 Unstaged Changes
git restore .
# 等价于
git checkout -- .
```

switch 的例子：
```shell
# 切换分支
git switch test
# 等价于
git checkout test

# 创建并切换分支
git switch -c test
# 等价于
git checkout -b test
```

参考链接：
* [TIL — git switch & git restore](https://pawelgrzybek.com/til-git-switch-git-restore/)

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
