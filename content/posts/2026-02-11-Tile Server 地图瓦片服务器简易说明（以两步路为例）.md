---
title: "Tile Server 地图瓦片服务器简易说明（以两步路为例）"
date: 2026-02-11T22:30:32+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: false
tags: ["地图", "运动", "生活"]
categories: ["运动"]
series: ["tech"]
slug: "20260211-map-tile-server"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

**注意**：由于本篇文章会以两步路的图源配置作为案例，所以将只涉及到**非矢量**的地图瓦片服务器说明

我们以 [OpenTopoMap 的瓦片服务器](https://opentopomap.org/about)为例：

```txt
https://{a|b|c}.tile.opentopomap.org/{z}/{x}/{y}.png
```

解释：
- 在地图瓦片服务器的 URL 模板中，`{a|b|c}` 代表子域名；在此例中，表示地图应用可以并发请求 `a.tile.opentopomap.org`、`b.tile.opentopomap.org`、`c.tile.opentopomap.org` 以加速地图的加载
- `{z}` 表示缩放的级别
- `{x}` 和 `{y}` 编码地图区域

例如，我们可以通过浏览器的开发者工具，并通过缩放地图来确认 `{z}` 的可能取值范围、以及瓦片服务器的地址：

![Windy 的地图瓦片服务器](/img/posts/20260211-t1.jpg "Windy 的地图瓦片服务器")

![OpenTopoMap 的地图瓦片服务器](/img/posts/20260211-t2.jpg "OpenTopoMap 的地图瓦片服务器")

以两步路的图源编辑为例进行说明（详细说明见图中）：

![两步路的图源编辑说明](/img/posts/20260211-t0.jpg "两步路的图源编辑说明")

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
