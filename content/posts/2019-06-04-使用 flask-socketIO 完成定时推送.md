---
title: "使用 flask-socketIO 完成定时推送"
date: 2019-06-04T09:32:05+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["Python", "CSDN备份"]
categories: ["Python"]
series: ["tech"]
slug: "20190604-tech-flask-socketio"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

flask 是一款 python 的 web 框架，而 socketIO 则是一款用于实时通信的高级封装库，它可以智能选择 websocket、长轮询等方式进行双工通信。


## 1. 面向事件驱动的 socketIO

socketIO 原本是一款 node.js 库，也有众多其他语言的实现版本。socketIO 的思想是使用事件驱动，通过不同的事件收发来完成通信。而 flask-socketIO 同样也是面向事件驱动的。

## 2. flask 使用 flask-socketIO 完成定时推送

定时推送，是我们使用 websocket 的主要目的之一，flask 完成定时推送，主要使用后台的线程来完成。

在 flask-socketIO 的 github [示例代码](https://github.com/miguelgrinberg/Flask-SocketIO/tree/master/example)中，就有这样的逻辑。

下面是本文提供的一种实现方式：

```python
from threading import Lock
from flask import Flask
from flask_socketio import SocketIO
import time
import json
import random

ASYNC_MODE = None
NS_DATA_PUSH = '/ws/dataPush'
RANDOM_MIN = 0
RANDOM_MAX = 50

app = Flask(__name__)
app.config['SECRET_KEY'] = 'secret!'
socketio = SocketIO(app, async_mode=ASYNC_MODE)

thread = None
thread_lock = Lock()
connections = 0


def background_data_push():
    global connections
    while connections > 0:
        with open('./data/pushData.json', 'r', encoding='utf-8') as f:
            obj = json.load(f)

        app.logger.info("DateTime: " + str(time.asctime()))
        socketio.emit('push_data', obj, namespace=NS_DATA_PUSH)
        socketio.sleep(1)


@socketio.on('connect', namespace=NS_DATA_PUSH)
def handle_connect():
    global thread, thread_lock, connections
    with thread_lock:
        connections += 1
        app.logger.info('Connection +1')
        if (thread is None) or (not thread.is_alive()):
            thread = socketio.start_background_task(background_data_push)


@socketio.on('disconnect', namespace=NS_DATA_PUSH)
def handle_disconnect():
    global thread, thread_lock, connections
    with thread_lock:
        connections -= 1
        app.logger.info('Connection -1')
        if connections == 0:
            thread.join()


if __name__ == '__main__':
    socketio.run(app, debug=True)

```

## 3. 相关链接

* [flask-socketIO Docs](https://flask-socketio.readthedocs.io/en/latest/)
* [socketIO Portal](https://socket.io/)
* [MDN - WebSocket](https://developer.mozilla.org/zh-CN/docs/Web/API/WebSocket)


---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
