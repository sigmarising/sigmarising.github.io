---
title: "Git Worktree 简易使用指南"
date: 2026-02-25T22:19:28+08:00
header_img: "/img/bg-tech.jpg"  # can be deleted
short: true
toc: true
tags: ["git"]
categories: ["git"]
series: ["tech"]
slug: "20260225-tech-git-worktree"  # final real url, recommend: start by date, follow lower case words with hyphen splitter. E.g., `20230316-text-title`
---

## 1. 为什么我们需要 git worktree

Git Worktree 能够为当前的代码仓库创建多个“仓库分身”

- 从而达成：**打开 N 个代码编辑器，在 N 个仓库分身中**，**同时**操作 N 个分支
- 而非：**打开 1 个代码编辑器，在 1 个仓库中**，**分时**操作 N 个分支

这个功能**特别适合于使用多路 AI Coding Agent 进行多个 feature 的同时开发**。

> Cursor 内置了自动在 Worktree 中本地编辑的功能，但其会把仓库分身（Worktree）建立在 `~/.cursor/worktrees/` 下

## 2. Git Worktree 简单用法

```shell
# 查看所有的 worktree
git worktree list

# 在路径 `../name-of-repo` 下，新建一个仓库分身 worktree
# 并在其中以 branch `main` 为基础，创建新的 branch `featA`
git worktree add -b featA ../name-of-repo main

# 删除已有的 worktree
git worktree remove ../name-of-repo

# 也可以直接 `rm -rf ../name-of-repo`
# 然后再运行 prune 进行修剪
git worktree prune
```

## 3. Git Worktree 的 Branch 说明

所有的 worktree 共享同一套 branch，因为所有的 worktree 本质上归属于同一个 git repo（使用同一个 `.git` 目录）。在 worktree A 中新建的 branch，在 worktree B 中同样可以看到（一并共享的也包括 tags、remote tracking branches、stash 等）。

但 worktree 有一个重要限制：**同一个 branch 不能同时被多个 worktree checkout**。\
即，**每个 branch 同一时间只能被一个 worktree 所占用**。

## References

- [Git Doc - worktree](https://git-scm.com/docs/git-worktree)
- [git worktree 簡單介紹與使用](https://louis383.medium.com/git-worktree-%E7%B0%A1%E5%96%AE%E4%BB%8B%E7%B4%B9%E8%88%87%E4%BD%BF%E7%94%A8-876897c797bf)

---

> 版权声明：本文遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) 版权协议，转载请附上原文出处链接和本声明。
>
> Copyright statement: This article follows the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement. For reprinting, please attach the original source link and this statement.
