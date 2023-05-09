---
title: "Maven 相关：mvn clean package 与 mvn clean install 的区别"
date: 2019-03-04T22:08:41+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: false
tags: ["Java", "CSDN备份"]
categories: ["Java"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20190304-mvn-clean-xxx"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

```shell
mvn clean package
mvn clean install
```

以上两条命令，表面上看做了相同的工作，实际上却略有区别。

* `mvn clean package`：删除目标文件夹、编译代码并打包 
* `mvn clean install`：删除目标文件夹、编译代码并打包、将打好的包放置到本地仓库中

> [参考资料：How are “mvn clean package” and “mvn clean install” different?
](https://stackoverflow.com/questions/16602017/how-are-mvn-clean-package-and-mvn-clean-install-different)


---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
