---
title: "防止 git merge 丢失代码（谨防 three way merge 的异常行为）"
date: 2021-08-31T16:09:27+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["git", "CSDN备份"]
categories: ["git"]
series: ["tech"]
slug: "20210831-tech-git-merge"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

Git 工具在执行 merge 操作时，**为了尽最大可能去自动的处理，所以使用了 three way merge 的方式作为了其 merge 手段**。然而，这种 Git 尽最大可能自动化的处理在一些时候会造成十分令人困惑的问题。本文旨在指出问题并提供一些避免手段。

## 1. Git's Three Way Merge
在 branch-A 和 branch-B 进行 merge 操作时，若两个分支均包含有 Main.py 文件，Git 会使用 Three way merge 的方式进行合并处理。简单来说，就是会找到 branch-A HEAD Commit 和 branch-B HEAD Commit 的公共根节点 Commit-Base，综合对比来进行合并。

详细内容可以参考 [Git三路合并算法(Three Way Merge)](https://marsishandsome.github.io/2019/07/Three_Way_Merge)，本文不再赘述原理部分。

> 为了方便讨论，我们不会涉及多个公共 Base Commit 节点的复杂案例。

## 2. 案例说明（Merge 造成了代码丢失而引发异常）

我们制造一个场景，让我们的 Git 分支情况如下：
![Git Branches](/img/posts/976753c1f0e2423ab68808689646cc67.png "Git Branches")

在 master 分支，文件如下：
![On Master](/img/posts/4898ea7cbe74467bb605a1b05ce6d46f.png "On Master")

dev_a 的修改为：
![dev-a](/img/posts/db3aff5ab6574227a3e5dbbc655a3b95.png "dev-a")

dev_b 的修改为：
![dev-b](/img/posts/2e9fd88c9ba14e26b589a22ad09a1845.png "dev-b")

### 2.1 场景描述

* Main.py 为主要的工作文件
* 开发者 A 在 dev_a 下新增了 get_json_thing 的方法
* 开发者 B 在 dev_b 下新增了主函数的打印，同时删掉了一些没有使用的 import 语句

### 2.2 Merge and Boom

现在开发者 A 想要通过 merge dev_b 来把开发者 B 的打印日志合并进来，于是他执行：
```shell
git merge dev_b
```

现在 merge 成功了，但是他发现自己的代码不 work 了，因为 Git 自作聪明的 Three Way Merge 将 `import json` 删掉了：

![result](/img/posts/321a60794dea45558571e67511fc94a2.png "result")

结合在第一节的参考链接，我们很容易理解 Git 这次 Merge 的操作原理，**因为不涉及到冲突，Git 把 dev_a 和 dev_b 相对于 master 的更改全部采纳了。**

***这只是一个非常非常简单的例子而已，在实际的开发工作中，我们的代码要复杂的多，而这样的情况稍稍复杂一些，我们很可能因为 Git Merge 的自动行为造成大量的代码丢失。***

## 3. 如何避免

### 3.1 避免多个人在多个 Branch 对同一个文件进行大量修改

只要不涉及到在不同的 Branch 对同一个文件进行修改，**Git 的 Three Way Merge 自动行为将不会自动删掉我们的潜在有用的代码。**

### 3.2 主动造成 Conflict 并使用高级 Git 工具手动 Merge

我们将我们的案例稍稍改动，使其在 Merge 的时候造成冲突：
![Conflict](/img/posts/84188596c0144c90afba01ce6c1bc2f5.png "Conflict")

**值得注意的是，Git 只会将 Conflict 的部分进行标注，它能够 Three Way Merge 自动解决的部分仍然无法标注出来。我们仍然无法自行决定要保留来自两个分支的哪些更改。**

而来自 JetBrains IDE 和 Visual Studio 等的**高级 Git 工具均可以将两个 Branch 的所有的变更进行显示，自行决定要保留各个分支的哪部分更改**：
![JetBrains IDE](/img/posts/964acbbd25294d678edcfbb19729b43a.png "JetBrains IDE")

![Visual Studio](/img/posts/c4bdc73649084160ab1ca72fb09f4b01.png "Visual Studio")

### 3.3 提前保留最终结果

我们可以利用 diff&merge 工具（Two Way Merge）提前手动决定一份最终版本的结果，然后在合并结束后覆盖合并后的文件即可。

### 3.4 最理想但是无解的方法

最理想情况下，我们应该可以控制 Git Merge 使其强制使用 Two Way Merge 的方式合并，制造大量冲突让我们手动判定（***多做人工操作，不相信机器***）。但是来自 [Git 的文档](https://git-scm.com/docs/git-merge) 显示**并不支持**这种方式。

所以如果你有更好的方案，欢迎探讨！

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
