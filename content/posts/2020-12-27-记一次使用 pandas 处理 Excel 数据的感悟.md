---
title: "记一次使用 pandas 处理 Excel 数据的感悟"
date: 2020-12-27T22:56:53+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["开发", "CSDN备份"]
categories: ["开发"]
series: ["tech"]
slug: "20201227-tech-meme-pandas"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

Pandas 是 Python 的知名数据处理库。前几天由于个人的需求，需要处理一下 Excel 数据，遂准备使用 pandas 处理。

## 1. 引入 pandas 时的问题

在 pip 安装之后，使用 `import pandas as pd` 便出现了问题。

```txt
The current Numpy installation fails to pass a sanity check due to a bug in the windows runtime
```

问题解决方法也很简单，降级 numpy 便可……

[参考链接: StackOverflow](https://stackoverflow.com/questions/64729944/runtimeerror-the-current-numpy-installation-fails-to-pass-a-sanity-check-due-to)

## 2. 读取 Excel 时的问题

接下来我使用 `read_excel` 函数，读取 excel 文件，注意官方的文档说是支持 `.xslx` 文件的

> Supports xls, xlsx, xlsm, xlsb, odf, ods and odt file extensions read from a local filesystem or URL. Supports an option to read a single sheet or a list of sheets.
>
> [Docs: pandas.read_excel](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_excel.html)

按照命令行输出提示，pip 安装了 xlrd 包，问题又出现了：
```txt
xlrd.biffh.XLRDError: Excel xlsx file； not supported
```

查阅发现，xlrd 包在 v2 版本取消了 xlsx 文件的支持，需要换解析引擎为 openpyxl……

[参考连接: pandas无法打开.xlsx文件，xlrd.biffh.XLRDError: Excel xlsx file； not supported](https://blog.csdn.net/weixin_44073728/article/details/111054157)

## 3. 个人感悟

开源社区虽然意味着广大的 support，但并不意味着拥有很好的 stable。以 Gephi 为例，这是一款极其优秀的处理图数据的可视化软件，功能齐全丰富，但是小 bug 很多，社区已经不更新很多年了。

为什么众多大公司都会维护自己的基础架构库，改出一套自己的轮子呢？Stable 和可控绝对是很大的因素。我使用 pandas 读取 excel 就遇到了上述两个烦人的问题，换成小白编程者很容易直接 freeze 处理进程而影响后续工作。

开源，虽然是一种很好的精神，但开源绑架是没有任何益处的。开源社区本身就良莠不齐，本来大家都是用爱发电，寄过高希望于其中没有任何意义（更不要指望社区去为你加 feature）。理解各个公司维护自己的基础架构库，造自己的轮子，不论别的，就 stable 和 feature 可控就值得这些付出了。

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
