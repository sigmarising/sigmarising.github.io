---
title: "利用 Python 自带的 TCPServer 编写 TCP 接收服务器"
date: 2019-12-14T13:38:09+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["Python", "CSDN备份"]
categories: ["Python"]
series: ["tech"]
slug: "20191214-tech-python-tcp"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

Python3.6+ 自带了 TCPServer 以便于更快编写 TCP 服务器，同时 Python 也提供了多线程的 Mixin 以提供多线程支持。

由于每个 TCPServer 只能服务于一个 TCP 连接，而 **Python 的多线程 Mixin 并不限制最大连接数**，所以可能会导致线程过度增长。

本文介绍一种简单的自制线程池方法的 Python TCP 服务器。

## 代码
`settings.py`
```python
BUF_SIZE = 1024
SERVER_PORT = 8973
THREAD_WORKERS_NUM = 10
```

`server.py`
```python
from socketserver import BaseRequestHandler, TCPServer
from threading import Thread
import settings
import logging

# 自定义的日志
logging.basicConfig(
    level=logging.INFO,
    format='[%(levelname)s] %(asctime)s %(filename)s: %(message)s',
    datefmt='%Y-%m-%d %A %H:%M:%S',
    filename='socket.log',
    filemode='a',
)


class ReceiveHandler(BaseRequestHandler):
    def setup(self):
        logging.info("Connect from: " + str(self.client_address))

    def handle(self):
        try:
            while True:
                msg = self.request.recv(settings.BUF_SIZE)
                if msg:
                	# 接收处理
                    pass
                else:
                    break
        except Exception as e:
            logging.warning('Exception is: ' + str(e))

    def finish(self):
        logging.info("Disconnect from: " + str(self.client_address))


if __name__ == '__main__':
    try:
        logging.info("Start Server !")
        socket_serve = TCPServer(('', settings.SERVER_PORT), ReceiveHandler)
        for i in range(settings.THREAD_WORKERS_NUM):
            t = Thread(target=socket_serve.serve_forever)
            t.daemon = True
            t.start()
        socket_serve.serve_forever()
    except KeyboardInterrupt:
        socket_serve.shutdown()
        logging.info("Stop Server !")
```

## 参考链接

* [创建TCP服务器](https://python3-cookbook.readthedocs.io/zh_CN/latest/c11/p02_creating_tcp_server.html)
* [python logging 日志输出 学习笔记 时间格式化](https://blog.csdn.net/z_johnny/article/details/50812878)


---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
