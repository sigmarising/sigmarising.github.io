---
title: "Python - 编写模块时 获取当前路径 __file__ 与 getcwd()"
date: 2018-10-27T11:10:17+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["Python", "CSDN备份"]
categories: ["Python"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20181027-python-path"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

# Python - 编写模块时 获取当前路径 \_\_file__ 与 getcwd()

编写 Python 模块时，我们有时候需要去获取模块文件的路径，进行相关操作。

本文将介绍合理的当前路径获取方法。

## 不要使用 getcwd() 方法

`os.getcwd()` 方法用于返回当前**工作目录**，所以在其他文件 import 了我们的包之后，`os.getcwd()` 返回的是：**当前正运行的 python 文件目录**。

## 使用 \_\_file__ 获取当前路径

`__file__` 表示显示文件当前的位置：

* 如果当前文件包含在 `sys.path` 里面，那么 `__file__` 返回一个相对路径
* 如果当前文件不包含在 `sys.path` 里面，那么 `__file__` 返回一个绝对路径

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
