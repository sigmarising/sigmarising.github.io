---
title: "配置你的 Linux/MacOS 终端"
date: 2018-10-27T10:35:15+08:00
subtitle: "oh-my-zsh + spf13-vim"  # can be deleted
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["开发", "CSDN备份"]
categories: ["开发"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20181027-config-shell"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

在 Linux 系或者 MacOS 上，你可以通过使用 ZShell(zsh) 代替默认的 Bash，来获得更好的终端体验。

## Step1. 安装 zsh 到本机

不同的 Linux 发行版安装的方法（MacOS 自带 zsh）并不一样，但需要做的总共有两件事：

1. 安装 zsh 到本机上
2. 替换 bash，让 zsh 成为默认的终端（**你可能需要重启生效**）

```shell
# 替换 shell 指令
chsh -s /bin/zsh
```

之后，你可以通过编辑 `~/.zshrc` 配置环境变量。

## Step2. 安装 powerline/fonts

[GitHub仓库地址：powerline/fonts](https://github.com/powerline/fonts)

安装方法，摘自 GitHub `Readme.md`

```bash
# clone
git clone https://github.com/powerline/fonts.git --depth=1
# install
cd fonts
./install.sh
# clean-up a bit
cd ..
rm -rf fonts
```

> **注意**：不要使用 apt-get 方法去安装，一定要通过运行 `install.sh` 的方法安装，否则容易出现图标显示问题

## Step3. 安装 oh-my-zsh 以及 spf13-vim

[GitHub仓库地址：oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)\
[GitHub仓库地址：spf13-vim](https://github.com/spf13/spf13-vim)

通过运行 `Readme.md` 给出的命令即可安装：

```bash
# oh-my-zsh
# 需要事先安装 curl git
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

# spf13-vim
# 需要事先安装 curl git vim
# 安装过程较慢，过程中需要输入 Github 账号密码以继续安装
curl https://j.mp/spf13-vim3 -L > spf13-vim.sh && sh spf13-vim.sh
```

## Step4. 安装 zsh 的实用插件

[GitHub仓库地址：zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)\
[GitHub仓库地址：zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)

安装摘自 GitHub `INSTALL.md` ：

```bash
# 进入 oh-my-zsh 的自定义插件目录
cd ~/.oh-my-zsh/custom/plugins

# git clone 两个插件
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

## Step5. 配置 .zshrc 文件

执行 `vim ~/.zshrc`，修改 `.zshrc` 文件如下部分：

```bash
ZSH_THEME="agnoster"

plugins=(
  zsh-autosuggestions
  zsh-syntax-highlighting
  ......
)
```

保存后退出，执行 `source ~/.zshrc`，使配置文件生效。

## Step6. 命令行软件配置

无论使用的哪种命令行软件，都需要最后修改配置：

* 默认编码 UTF-8
* 字体修改为 Powerline 字体：***Hack***（十分推荐的编程字体！）

---

至此，大功告成，享受你的 terminal 吧！

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
