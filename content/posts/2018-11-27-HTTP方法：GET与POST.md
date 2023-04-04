---
title: "HTTP方法：GET与POST"
date: 2018-11-27T12:05:13+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["开发", "CSDN备份"]
categories: ["开发"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20181127-get-post"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

在 HTTP 中，最常用的两种方法就是 GET 和 POST。

本文将会介绍两者的区别。

## GET

查询字符串（名称/值对）是在 GET 请求的 URL 中发送的。

* GET 请求可被缓存
* GET 请求保留在浏览器历史记录中
* GET 请求可被收藏为书签
* GET 请求不应在处理敏感数据时使用
* GET 请求有长度限制
* GET 请求只应当用于取回数据

## POST

查询字符串（名称/值对）是在 POST 请求的 HTTP 消息主体中发送的。

* POST 请求不会被缓存
* POST 请求不会保留在浏览器历史记录中
* POST 不能被收藏为书签
* POST 请求对数据长度没有要求

## 对比

|项目|GET|POST|
|:-:|:-:|:-:|
|后退/刷新|无影响|数据会被重新提交|
|收藏为书签|可以|不可以|
|能否缓存|可以|不可以|
|历史记录|参数会保留|参数不会保留|
|数据长度|受限（URL 2048个字符）|无限制|
|数据类型|只允许 ASCII 字符|无限制（允许二进制数据）|
|安全性|差|好|
|可见性|在 URL 中|不在 URL 中|

## 参考链接

[HTTP 方法：GET 对比 POST
](http://www.w3school.com.cn/tags/html_ref_httpmethods.asp)

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
