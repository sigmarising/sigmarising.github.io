---
title: "关于 Gerrit Code Review 的那些 Label"
date: 2018-11-02T17:53:33+08:00
subtitle: "-2 -1 +1 +2"  # can be deleted
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["git", "CSDN备份"]
categories: ["git"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20181102-gerrit-code-review-label"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

初次使用 Gerrit，你是否弄不清楚什么是 -2 -1 +1 +2 ？此文缩略翻译自 Gerrit 官方文档，以供参考。

## 标签：代码评审

标签值的范围如下：

* **-2**：代码有严重的问题，绝对不可以进行合并。***注意：Any -2 blocks submit***
* **-1**：代码看上去不太好，但需要其他评审人再次确认
* **0**：还没仔细去看代码
* **+1**：代码看上去没问题，但需要其他评审人再次确认
* **+2**：代码看看上去不错，同意合并。***注意：Any +2 enables submit***

值得注意的是，-2 -1 0 +1 +2，都是针对于**评审者自身的意见**而言的，并不代表所有人的意见。

## 英文原版缩略

The range of values is:
* **-2:** This shall not be merged
* **-1:** I would prefer this is not merged
* **0:** No score
* **+1:** Looks good to me, but someone else must approve
* **+2:** Looks good to me, approved

官方文档还提供了 *Label:Verified* 的说明，此处不详细叙述，可查看官方文档 [Gerrit Code Review - Review Labels](https://review.openstack.org/Documentation/config-labels.html) 了解更多信息。

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
