---
title: "配置文件备份：nginx+uwsgi+django"
date: 2019-10-01T17:37:32+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["Linux", "CSDN备份"]
categories: ["Linux"]
series: ["tech"]
slug: "20191001-tech-backup-conf"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

现在流行的 Web 开发模式多为前后端分离开发，即前端使用 Angular、React、Vue 框架开发并打包为静态文件，部署到 Apache、Nginx 服务器，同时对路由做好对应的配置。而后端服务仅提供 RESTful、WebSocket 服务等。

互联网上关于 nginx、uwsgi 的配置文件教程各不相同，有些并不能起作用。笔者通过亲自部署相关的 Web 页面，并在本文中列出对应的配置文件信息。

![本文大致架构](/img/posts/20191001173206195.png "本文大致架构")

## 1. 前提条件

安装好相关的工具，例如：Nginx、Python、Uwsgi 等（Linux 环境下）。

**P.S. 本文并不会提供详细的建立网站说明，只是对配置文件进行相关的备份。如需要建立网站，可以参阅文末的参考链接。**

## 2. Nginx 配置文件

在 `/etc/nginx` 目录下，结构大致如图：

![nginx dir](/img/posts/20191001171622856.png "nginx dir")

其中主要配置文件是 `nginx.conf`，这个文件中包含如下指令：

![include 指令](/img/posts/20191001171724991.png "include 指令")

所以，我们只需要在 `conf.d` 文件夹下新建配置文件，就可以达到自定义配置的目的了：

![示例](/img/posts/20191001171849470.png "示例")

这里我新建了两个配置文件，一个是对 Vue 打包后的 Web 页面提供服务，另一个是连接 Uwsgi 的后端。

### 2.1 Web 页面服务

```conf
server {
	listen 3000;
	server_name icdm2019;
	charset     utf-8;
	location / {
		root /root/zhangyun/web/icdm-dssc-2019-frontend/dist;  # vue 打包后静态文件存放的地址
		index index.html; # 默认主页地址

		location /api {
			proxy_pass http://127.0.0.1:8000; # 代理接口地址
			include  /etc/nginx/uwsgi_params;
		}
	}
}
```

这里在 3000 端口提供页面服务，同时对 RESTful API 提供了跨域访问的支持。

### 2.2 后端服务

```conf
upstream django {
    server 127.0.0.1:8888; # for a web port socket (we'll use this first)
}

server{
    listen 8000;
    server_name icdm2019django;
    charset utf-8;
    client_max_body_size 75M;  #上传文件大小限制

    # 动态文件交给uwsgi处理
    location /api {
        uwsgi_pass 127.0.0.1:8888;
        include /etc/nginx/uwsgi_params;
    }
}
```

在此处，Nginx 在 8000 端口连接到 8888 端口的 uwsgi 服务。

## 3. uwsgi 配置

```ini
[uwsgi]
home = /home/workspace/judicature-subject2-backend/venv # python 虚拟环境所在目录
chdir = /root/zhangyun/web/icdm-dssc-2019-backend/ # 后端 manage.py 所在目录
wsgi-file=/root/zhangyun/web/icdm-dssc-2019-backend/icdm2019/wsgi.py # 定位到 wsgi.py
module = icdm2019django.wsgi:application
master = True
processes = 4
max-requests = 5000
harakiri = 60
socket = 127.0.0.1:8888
uid = root
gid = root
pidfile = /home/icdm2019uwsgi/master.pid
daemonize = /home/icdm2019uwsgi/mysite.log
logto = /home/icdm2019uwsgi/uwsgi.log # log 文件
vacuum = True
py-autoreload=1 # 自动重载 Py 文件
```

这样，uwsgi 在 8888 端口提供服务。

## 4. 常用指令

```shell
# 检查 nginx 配置文件是够有错误
nginx -t

# 重启nginx
service nginx restart
# 或者
nginx -s stop
nginx -c /etc/nginx/nginx.conf

# 查看uwsgi进程
ps -aux | grep uwsgi

# 正常关闭uwsgi进程
uwsgi --stop /home/mysite_uwsgi/master.pid

# 强制关闭全部uwsgi进程
ps -aux | grep uwsgi |awk '{print $2}'|xargs kill -9

```

## 5. 参考链接

[如何将django项目用Nginx部署到服务器?](https://www.zhihu.com/question/22850801/answer/656762026)


---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
