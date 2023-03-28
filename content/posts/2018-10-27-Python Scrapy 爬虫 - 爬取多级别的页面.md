---
title: "Python Scrapy 爬虫 - 爬取多级别的页面"
date: 2018-10-27T10:50:06+08:00
subtitle: "多级页面的 Scrapy 爬取策略"  # can be deleted
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["Python", "CSDN备份"]
categories: ["Python"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20181027-scrapy-pages"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

互联网中众多的 scrapy 教程模板，都是爬取 下一页$\rightarrow$下一页形式的，很少有 父级$\rightarrow$子级 的说明。

本文介绍一些使用 scrapy 爬取三级网页的注意事项。

## 逐级别传递 item

如何将 item 的信息，从父级传送到子级，是多级别爬取的最重要部分。

Scrapy 用 `scrapy.Request` 发起请求时，可以带上 `meta={'item': item}`，把之前已收集到的信息传递到新请求里。\
在新请求里用 `item = response.meta('item')` 接受过来，item 就可以继续添加新的收集的信息了。

> 参考链接：[Python Scrapy多层爬取收集数据](https://blog.csdn.net/ygc123189/article/details/79160146)

**注意**：meta字段的方法是**浅拷贝**，并非深拷贝，（[可参考官方文档](https://doc.scrapy.org/en/latest/topics/request-response.html#request-objects)），所以如果 item 有多个字段时，要么在meta中多字段表示，要么使用深拷贝方法。

```python
# 使用 scrapy 在多个 parse 中自上而下逐级传递 item 的方法
# method 1
import copy

yield scrapy.Request(
    url=next_url, 
    meta={
        'item': copy.deepcopy(item_before)
    }, 
    dont_filter=True, 
    callback=self.next_parse
)

# method 2
yield scrapy.Request(
    url=next_url, 
    meta={
        'thing1': thing1_before,
        'thing2': thing2_before
    }, 
    dont_filter=True, 
    callback=self.next_parse
)
```

##  是否需要进行 **url去重操作？**

如果二级页面的 url 是根据某内容来定义 *url路径* 的，因此会存在很多重复的 二级url，需要不去重操作。

> 去重机制：`scrapy.Request()` 的参数 `dont_filter` 默认是 `False`（去重）。\
> 每 yield 一个 `scrapy.Request()`，就将 url参数 与调度器内已有的 url 进行比较，如果存在相同 url 则默认不入队列，如果没有相同的 url 则入队列，\
> 如果想要实现不去重效果，需要将 `dont_filter` 改为 `True`
> 
> 来自参考链接：[spider爬取多级url](https://blog.csdn.net/loner_fang/article/details/81031075)

## scrapy selector 的 extract 与 extract_first 方法

* `extract` 以列表形式（记此列表为`a`）返回选择器中的 data 字段，\
  `extract_first` 则返回上述列表`a`中的第一个元素（多为字符串）
* 通常 `extract` 得到的列表中，只有一个元素，所以往往用 `extract_first` 即可。\
  但若 `extract` 得到的列表中有多个元素，则需要使用 `''.join(a)` 得到具体的字符串信息。
* `' xxx '.strip()` 可以用于去掉头尾空白字符

## 编写 pipeline 注意事项

* 首先需要在设置中启动pipeline，才可以有效果
* mkdir创建单级目录，makedirs创建多级目录
* 需要首先`dict(item)`
* 打开文件时
    * w 只可以写文件
    * w+ 可读可写，打开后立即清空
    * r 只可以读文件
    * r+ 可读可写
    * encoding 保存中文必选
* 写json时，注意使用`"`而非`'`
* `json.dump(text, f, ensure_ascii=False, indent=4)`可保存中文，并格式化json

## 启用 scrapy 日志功能

详细见参考链接

> [Scrapy logging settings](https://doc.scrapy.org/en/latest/topics/logging.html#logging-settings)


---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
