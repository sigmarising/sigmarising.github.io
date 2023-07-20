---
title: "理解 RESTful API"
date: 2019-03-24T18:09:31+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["开发", "CSDN备份"]
categories: ["开发"]
series: ["tech"]
slug: "20190324-tech-restful"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

REST（REpresentational State Transfer），即“表现层状态转移”，最早出现在 Roy Thomas Fielding （参与设计了 HTTP 协议、Apache 服务器）的博士论文中（2000年）。

> 论文地址：[Architectural Styles and
the Design of Network-based Software Architectures](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)

RESTful 是一种 API 的设计风格，其相比 SOAP 和 XML-RPC 更为简洁易用，现已经得到大范围应用。

## 1. REST and RESTful API

REST 描述的是在网络中 client 和 server 的一种交互形式，但 REST 本身不实用，实用的是如何设计 RESTful API（REST风格的网络接口）。

## 2. RESTful API 的设计

### 2.1 资源 URI 的设计规则

URI 中只使用**名词**（一般是复数形式）来指定资源，原则上**禁止使用动词**。API 应部署到**专有域名**下，可以考虑加入 API 的**版本**。

API 应支持对于 **`{id}`** 单个资源的操作。

```python
https://api.test.com/v1/books 		# 一组资源
https://api.test.com/v1/books/137	# 单个资源 

# 错误的例子
https://api.test.com/v1/getBooks	# API 中出现了动词
```

### 2.2 使用 HTTP 的请求方法表示 CRUD 操作

* **GET**：获取资源
* **PUT**：更新资源
* **POST**：新建资源
* **DELETE**：删除资源

> 值得注意的是，**RESTful 并没有“正式”的标准**，所以不同 Web 应用中，HTTP 动词代表的语义可能有所差异，但大致都是相似的。

### 2.3 表现形式 and 状态码

 Server 和 Client 之间使用某种表现形式来传递资源，通常是 `json`，也可以是其他形式。

通过 HTTP 的状态码，可以直观反映结果。

### 2.4 前后端分离

Web 端不必使用 PHP、JSP、ASP 架构，改为前端渲染、前端路由（Angular、React、Vue）。**Web 端和 Server 只使用 RESTful API 来传递数据和改变数据状态**。

## 3. 总结
* 从 URI 可直观清楚要操作的对象
* 从 HTTP 方法可直观理解要做的操作
* 从状态码可直观了解运行结果

## 参考链接

* [RESTful API 设计指南](http://www.ruanyifeng.com/blog/2014/05/restful_api.html)
* [维基百科 - 表现层状态转换](https://zh.wikipedia.org/wiki/%E8%A1%A8%E7%8E%B0%E5%B1%82%E7%8A%B6%E6%80%81%E8%BD%AC%E6%8D%A2)
* [怎样用通俗的语言解释REST，以及RESTful？ - 覃超的回答](https://www.zhihu.com/question/28557115/answer/48094438)

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
