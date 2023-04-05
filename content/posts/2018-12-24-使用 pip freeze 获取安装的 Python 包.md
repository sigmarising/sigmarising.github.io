---
title: "使用 pip freeze 获取安装的 Python 包"
date: 2018-12-24T16:16:49+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["Python", "CSDN备份"]
categories: ["Python"]
series: ["tech"]   # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20181224-pip-freeze"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

有时，我们为了代码稳定、代码迁移等，需要获取当前 Python 工程依赖包的安装列表。这个列表要包括需要安装什么包、以及包的版本。这便是：`requirements.txt`。

## pip 使用 requirements.txt 安装

输入命令：
```bash
pip install -r requirements.txt
```
即可安装 `requirements.txt` 中的所有包（指定版本）。

## pip freeze

使用 `pip freeze` 会输出所有在本地已安装的包（但不包括 `pip`、`wheel`、`setuptools` 等自带包），若需要输出内容与 `pip list` 一致，需使用 `pip freeze -all`。

使用方法： 
```bash
pip freeze > requirements.txt
```

## 适用场合

由于 `pip freeze` 与 `pip list` 内容区别不大，所以，***若想要用其作为工程依赖包列表，一定要配合 Python 虚拟环境 `virtualenv` 使用。***

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
