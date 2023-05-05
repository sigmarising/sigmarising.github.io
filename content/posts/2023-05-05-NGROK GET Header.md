---
title: "NGROK GET Header"
date: 2023-05-05T23:02:52+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
tags: ["前端"]
categories: ["前端"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20230505-tech-ngrok-get-header"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

NGROK 是一个很好用的内网穿透工具，在向 NGROK host 的 Server 发送请求时，最好添加上 header：

```
"ngrok-skip-browser-warning": "true" 
```

否则在进行 GET 请求访问时，极大可能会存在 Response 异常（NGROK 浏览器警告页）的情况。

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
