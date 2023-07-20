---
title: "一文了解 conda 与 Python 的关系"
date: 2019-03-24T11:15:27+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["Python", "CSDN备份"]
categories: ["Python"]
series: ["tech"]
slug: "20190324-tech-conda-python"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

conda 是 2012 年发布的跨平台包管理软件，本文会对 conda 与 Python 的种种关系进行说明。

## 1. 什么是 conda ？

conda 是一个包管理器。值得注意的是，**它不仅仅是 Python 的包管理器，而是一个通用的包管理器，当初设计时被用来管理任何语言的包**。在目前来看，conda 环境中所有语言的包管理，都是为了 Python 而服务的。

## 2. Anaconda 与 Miniconda

**Anaconda 是一个 Python 的发行版**，内置了众多 Python 包和附加软件（pydata 生态圈里的软件），所以 Anaconda 自然内置了 conda。

**而 Miniconda 则提供了一个最小的 conda 安装环境**，十分干净轻巧。

## 3. conda 与 pip

conda 和 pip **均可以用来管理 Python 依赖包**，但二者也仅仅在包管理这个子集上有交集。

pip 和 conda 处理包的底层机制各不相同，pip 使用 wheels，conda 是二进制编排。conda 对于底层 c 代码的依赖处理的更好。

## 4. conda 与 virtualenv

conda 和 virtualenv **都可以创建虚拟环境**，进行 Python 运行环境的隔离。

你也可以在 virtualenv 中使用 conda，但强烈不建议混用两者。

## 使用建议

conda 和 pip、virtualenv 都是很好的工具，它们为不同的目的而存在，如何使用可根据个人喜好和需求而来。

***但请不要混用两者***：
* **要么持续使用 pip、virtualenv**
* **要么持续使用 conda**

## 参考链接

[关于conda和anaconda不可不知的误解和事实——conda必知必会](http://nooverfit.com/wp/%E5%85%B3%E4%BA%8Econda%E5%92%8Canaconda%E4%B8%8D%E5%8F%AF%E4%B8%8D%E7%9F%A5%E7%9A%84%E4%BA%8B%E5%AE%9E%E5%92%8C%E8%AF%AF%E8%A7%A3-conda%E5%BF%85%E7%9F%A5%E5%BF%85%E4%BC%9A/)

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
