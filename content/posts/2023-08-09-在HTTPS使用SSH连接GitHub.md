---
title: "在 HTTPS 使用 SSH 连接 GitHub"
date: 2023-08-09T19:24:58+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: false
tags: ["git", "开发"]
categories: ["开发"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20230809-tech-github-ssh-on-https"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

众所周知，我们访问 GitHub 时，若因某些原因 22 SSH 端口被干扰，那么我们将无法正常使用 Git 的各种操作访问 GitHub。好在 GitHub 官网提供了一种使用 SSH on HTTPS 的方式进行访问，方法也非常简单。

首先运行：
```shell
ssh -T -p 443 git@ssh.github.com
```

若访问正常，可以在 `~/.ssh/config` 中添加如下配置：
```conf
Host github.com
    Hostname ssh.github.com
    Port 443
    User git
```

如果不出意外，我们已经可以通过正常域名访问 GitHub 了：
```shell
ssh -T git@github.com
```

参考链接：[在 HTTPS 端口使用 SSH](https://docs.github.com/zh/authentication/troubleshooting-ssh/using-ssh-over-the-https-port)

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
