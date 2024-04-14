---
title: trash-cli工具使用
toc: true
date: 2021-02-04 23:42:54
tags:
- 命令行
- 知识碎片
categories:
- [知识碎片,命令行]
---

> 前言: 之前一直使用 `rm` 在命令行删除文件, 直到最近误删了重要的文件后, 就觉得 `rm` 不安全了. 之前有听说有一个命令行工具可以向桌面上删除内容一样, 可以先将文件放到 `trashcan`, 如果误删了重要的文件, 可以方便的找回. 经过一番查找, 找到了一个命令行工具, `trash-cli`. 

<!--more-->

# 初识 trash-cli

trash-cli 是一个用 Python 写的命令行工具. 可以实现文件的删除, 恢复等功能. 

> 具体的代码还没有去学习. 我认为这个代码还是值得一看的. 后面可能会更新这个博客, 跟进看代码的情况. 

# 安装 trash-cli

在 Mac 上安装 trash-cli 可以使用 `homebrew` 来安装, 安装代码如下: 

```shell
brew install trash-cli
```

由于最新的 trash-cli 依赖于 Python3.9, 所以使用 `HomeBrew` 安装后会自动安装 Python3.9, 不过 `HomeBrew` 很好的处理了不同版本的 Python 的隔离, 所以不用担心, 上面的命令会很好的处理依赖关系. 

# 使用 trash-cli

trash-cli 的使用很简单, trash-cli 有 4 个命令: 

```shell
# 删除文件或者文件夹
trash <files/directories> 
# 列出回收站中的文件
trash-list
# 恢复放入回收站的文件
trash-restore
# 删除回收站中的文件(彻底删除)
trash-rm
# 清空回收站
trash-empty <n>
```

其中 `trash-restore` 命令功能比较复杂, 注意事项如下: 

- 在 `/home/usr` 目录下会自动列出路径中包含 `/home/usr` 目录的所有被删除文件
- 在 `/path/to/other` 目录下回自动列出 `/path/to/other` 目录下曾被删除的文件
- 一般删除的文件都会在 `/home/usr` 下, 所以想恢复文件的时候可以直接使用 `trash-restore ~` 来列出来曾经所有被删除的文件
- 列出来文件目录之后, 再键入根据列出来的文件或文件夹对应的数字就可以恢复文件到对应的位置了

`trash-empty` 除了直接清空回收站之外, 还可以增加一个参数指定清空加入回收站超过 n 天的文件



