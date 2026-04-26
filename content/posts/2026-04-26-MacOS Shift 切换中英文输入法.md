---
title: "MacOS Shift 切换中英文输入法"
date: 2026-04-26T12:49:33+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["效率", "软件"]
categories: ["效率"]
series: ["tech"]  # should be ONLY ONE of the ["tech", "life", "review"]
slug: "20260426-tech-mac-shift-cn-en"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

## 背景

我一直对于 MacOS 使用大小写锁定键当作中英文切换按键感到十分的难绷，而更要命的是，这个大小写锁定切换输入法还有“苹果愚蠢的小聪明：防误触延时”，使得其切换体验更加雪上加霜。

以及随着搜狗输入法 MAC 版已经炸裂到广告满天飞（谁说 MAC 没有流氓软件的？），微信输入法 MAC 版太丑而且 BUG 也不少，我又回归了原生“干净”的输入法。

## 配置

为了让原生输入法（以及切换）稍微能“正常”使用，[Karabiner-Elements](https://karabiner-elements.pqrs.org/) 就变成了必须要安装的软件了。通过高级配置，让你的 Shift 独立按下时，自动映射到 Ctrl + Space 组合键，从而达到切换输入法的目的。

值得注意的是，所有新接入的外接键盘，都需要在 Karabiner-Elements 稍微配置一下。在通过 Karabiner-Elements 管理外接键盘后，系统设置里的修饰键会不起作用，需要在 Karabiner-Elements 中重新配置修饰键。

基于[此文章](https://www.onejar99.com/mac-keyboard-en-zh-switch-by-karabiner-elements/#step_3_%E8%A8%AD%E5%AE%9A_Shift_%E4%B8%80%E9%8D%B5%E5%88%87%E6%8F%9B%E7%9A%84%E8%A6%8F%E5%89%87_%E9%81%A9%E7%94%A8_Karabiner_15x%EF%BC%8C20250824_%E6%9B%B4%E6%96%B0)，给出两种配置文件：

右 Shift 切换策略：
```json
{
  "description": "Change R_Shift to control + space to switch EN/ZH",
  "manipulators": [
    {
      "type": "basic",
      "from": {
        "key_code": "right_shift",
        "modifiers": {
          "optional": ["any"]
        }
      },
      "to": [
        {
          "key_code": "right_shift",
          "lazy": true
        }
      ],
      "to_if_alone": [
        {
          "key_code": "spacebar",
          "modifiers": ["left_control"]
        }
      ]
    }
  ]
}
```

左 Shift 切换策略：
```json
{
  "description": "Change L_Shift to control + space to switch EN/ZH",
  "manipulators": [
    {
      "type": "basic",
      "from": {
        "key_code": "left_shift",
        "modifiers": {
          "optional": ["any"]
        }
      },
      "to": [
        {
          "key_code": "left_shift",
          "lazy": true
        }
      ],
      "to_if_alone": [
        {
          "key_code": "spacebar",
          "modifiers": ["left_control"]
        }
      ]
    }
  ]
}
```

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
