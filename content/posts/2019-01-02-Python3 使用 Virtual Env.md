---
title: "Python3 使用 Virtual Env"
date: 2019-01-02T12:42:08+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["Python", "CSDN备份"]
categories: ["Python"]
series: ["tech"]   # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20190102-python-virtual-env"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

开发 Python 项目时，建议在开发、生产环境下都使用虚拟环境来管理项目依赖。

**为什么要用虚拟环境**？随着 Python 项目增多，不同的项目会需要不同版本的 Python 库，并且同一个 Python 库不同版本可能并不兼容。

虚拟环境可以为每一个项目安装独立的 Python 库，这样就能隔离不同项目之间的 Python 库，也能够隔离项目与操作系统之间的 Python 库。

同时，`pip freeze` 命令只有在配合虚拟环境使用时，才能发挥真正的依赖管理作用。

**在 Python 3 中内置了用于创建虚拟环境的 `venv` 模块，建议使用其创建虚拟环境。**

## 1. 创建虚拟环境

对于一个工程而言，建议将 `venv` 建立在工程目录之下：

```bash
# 创建工程
mkdir project

# 进入工程目录
cd project

# 创建虚拟环境
python -m venv ./venv
```

这样便创建好了工程目录下的虚拟环境。

## 2. 启用虚拟环境

不同 OS 不同 Shell，启用虚拟环境的方法各不相同：

|  平台   |   Shell    |               命令               |
| :-----: | :--------: | :------------------------------: |
|  Posix  |  bash/zsh  |   `source <venv>/bin/activate`   |
|  Posix  |    fish    |   `. <venv>/bin/activate.fish`   |
|  Posix  |  csh/tcsh  | `source <venv>/bin/activate.csh` |
| Windows |    cmd     |  `<venv>\Scripts\activate.bat`   |
| Windows | PowerShell |  `<venv>\Scripts\Activate.ps1`   |

> `<venv>` 需修改为虚拟环境对应的路径

***成功启用后，命令行会带有前缀 `(venv)`。***

启用虚拟环境之后，便可以像平时使用 `pip` 或 `python` 一样安装或运行 Python 依赖包、代码了。

### 关于 Windows 系统上的 PowerShell

由于 Windows 系统的 PowerShell 默认不执行未信任的 ps1 脚本，对于开发人员而言，十分不便。**建议将 PowerShell 默认设置为可执行任意脚本。**

设置方法为，使用管理员身份启动 PowerShell，执行命令如下：
```shell
Set-ExecutionPolicy Unrestricted -Force
```

完成之后，便可以正常使用虚拟环境了。

> 参考链接：
>
> [virtualenv won't activate on windows - stack overflow](https://stackoverflow.com/questions/18713086/virtualenv-wont-activate-on-windows)
>
> [Set-ExecutionPolicy - Microsoft Docs](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-6)

## 3. 关闭虚拟环境

在命令行中输入：

```bash
deactivate
```

便可退出虚拟环境。

> 值得注意的是，这条命令在不同 OS 不同 Shell 上可用性不同：
> * 一般 Linux 系统默认支持 `deactivate` 命令
> * 对于 Windows 系统，可尝试执行脚本 `deactivate.bat` 或 `deactivate.ps1`

## 4. 关于第三方模块 virtualenv

最早，创建 Python 虚拟环境的模块，即第三方模块 virtualenv。从 Python 3.3 开始，virtualenv 的核心功能被集成到内置模块 venv 中。但如果想使用高级功能（如创建指定 Python 版本的虚拟环境），仍需要使用原版模块 virtualenv。

详细可见：
* [pypi - virtualenv](https://pypi.org/project/virtualenv/)
* [github - virtualenv](https://github.com/pypa/virtualenv)
* [docs - virtualenv](https://virtualenv.pypa.io/en/latest/)

> 实际上，virtualenv 的高级功能并不常用，而且**其创建不同 Python 版本的虚拟环境相比 Conda 环境易用性很差**。
> 
> **建议：有创建不同 Python 版本的环境需求时，直接使用 Conda 环境**

## 其他参考链接

* [Python3.6 Docs - venv](https://docs.python.org/3.6/library/venv.html)
* [Flask - Installation: Virtual environments](http://flask.pocoo.org/docs/1.0/installation/#virtual-environments)


---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
