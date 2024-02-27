---
title: "以新版 Mini Conda 的安装而引申的思考"
date: 2024-02-27T21:05:37+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: false
tags: ["Python"]
categories: ["Python"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20240227-tech-think-with-new-conda-install"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

前不久，突然想把 conda 拿起来玩玩，准备当作 Python Version Manager 来用。不过 Mini Conda 的安装似乎和几年前我实验时有一些比较大的区别了。

首先依据 conda 的官方文档 [Installing on Windows](https://docs.conda.io/projects/conda/en/latest/user-guide/install/windows.html) 和 [Conda init](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#conda-init) 可知，***Conda 为了避免对已有的环境造成影响，从而使得侵入性最小***，一是不再推荐将 conda 添加到系统 PATH 中（All User 安装时自动禁用此选项），二是如果要在默认 shell 中可访问 conda，可以通过 `conda init` 命令来创建各个 shell 的 profile hook 来达到在命令行使用的目的。而关于如何不自动激活 base 环境的方法可参见此[链接](https://stackoverflow.com/questions/54429210/how-do-i-prevent-conda-from-activating-the-base-environment-by-default)。

阅读文档之后，我的第一感受是：“可以啊 Conda，为了侵入性最小考虑挺多啊”。然而事与愿违，我在我的某台开发机上遇到了类似于[conda activate 激活虚拟环境失败原因](https://github.com/ZutJoe/blog/issues/4)的问题。简而言之就是，Conda Hook 注入之后，实则接管了 shell 的所有 prompt 进行处理，但它并没有考虑 Powerline 等特殊字体之类的各种情况。我不禁为之一震，**宣称侵入性最小，实则侵入性十足啊**。突然又想起来当年的[记一次使用 pandas 处理 Excel 数据的感悟](/posts/20201227-tech-meme-pandas/)。笑，还是老老实实用官方单版本 Python 吧。

P.S. 能别混用 pip 和 conda 就别混用这俩，否则你的 lib 环境将极难移植。

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
