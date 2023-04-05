---
title: "将 Python 2 代码转为 Python 3 代码"
date: 2019-01-22T16:52:17+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["Python", "CSDN备份"]
categories: ["Python"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20190122-python-2to3"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

`2to3` 是一个 Python 程序，它可以用来读取 *Python 2.x* 版本的代码，并使用一系列的**修复器 fixer**来将其转换为合法的 *Python 3.x* 代码。标准库中已经包含了丰富的修复器，这足以处理绝大多数代码。

## 使用 2to3 工具

### 2to3 的位置
`2to3` 通常会作为脚本和 *Python* 解释器一起安装，你可以在 *Python* 根目录的 `Tools/scripts` 文件夹下找到它。

> 不同系统，`2to3` 的位置也不一样，但一般均可在 *Python* 安装目录下找到

### 命令行调用

`2to3` 的基本调用参数是一个需要转换的文件或目录列表。对于目录，会递归地寻找其中的 *Python* 源码。它可以在命令行中使用 `2to3` 转换成 *Python 3.x* 版本的代码：
```shell
2to3 example.py
```
这个命令会打印出目标文件和源文件的 diff 信息。
* 传入 `-w` 参数，`2to3` 会把需要的修改写回到原文件中，同时对源文件备份
* 传入 `-w -n` 参数，会仅仅将修改写回到原文件中，而不备份源文件


## 参考链接

[2to3 - 自动将 Python 2 代码转为 Python 3 代码](https://docs.python.org/zh-cn/3.6/library/2to3.html)\
[2to3 - Automated Python 2 to 3 code translation](https://docs.python.org/3.6/library/2to3.html)


---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
