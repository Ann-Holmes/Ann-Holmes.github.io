---
title: 'vim指定swap,undofil,backup'
toc: true
date: 2021-02-05 11:14:08
tags:
- 软件
- Vim
- 配置
categories:
- [软件,Vim]
---

> 最近发现 Mac 上的 Vim 打开文件后总会留下各种以`~` 结尾的缓存文件, 删除原始文件后, 缓存文件依然存在, 这样污染文件夹很让人苦恼. 网上查阅一番之后, 发现了下面的解决方法. 

<!--more-->

Stack Overflow 上面有两种解决方法: 

1. 激进的禁用 Vim 生成缓存文件
2. 指定 Vim 缓存文件的存储位置

禁用 Vim 生成缓存文件很简单, 只需要在 Vim 的配置文件中引入下面语句即可: 

```shell
set nobackup
set noundofile
set noswap
```

指定 Vim 缓存文件存储位置, 也是需要修改配置文件即可: 

```shell
let &directory = expand('~/.vim/vimdata/swap//')

set backup
let &backupdir = expand('~/.vim/vimdata/backup//')

set undofile
let &undodir = expand('~/.vim/vimdata/undo//')

if !isdirectory(&undodir) | call mkdir(&undodir, "p") | endif
if !isdirectory(&backupdir) | call mkdir(&backupdir, "p") | endif
if !isdirectory(&directory) | call mkdir(&directory, "p") | endif
```

上面的配置会在 `.vim`文件夹下创建 `vimdata/swap`, `vimdata/backup`, `vimdata/undo` 文件夹. 并将对应的缓存文件缓存在创建的文件夹下. 这样做的好处是可以一直 undo 到这个文件最开始的时候. 

