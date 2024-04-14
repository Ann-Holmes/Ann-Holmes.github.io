---
title: Github Pages ➕ Hexo 搭建个人博客
toc: true
date: 2024-04-14 17:21:49
tags:
categories: [知识碎片,hexo]
---


<!--more-->

三年前因为一时兴起基于 Gitee ➕ Hexo 搭建了一个博客系统. 后续写了几篇博客就没再写了, 一方面是学习太忙碌了. 另一方面是 Gitee 后续出了审核政策, 基本上就夭折了, 我也不太敢在上面发表内容了. 

工作了一段时间, 感觉业余的时间还比较充足, 也比较想将自己的学习的心得和知识分享出来. 希望能够在互联网上有一点点小小痕迹. 于是根据之前 [Gitee ➕ Hexo 的记录内容](https://ann-holmes.github.io/2021/02/03/Gitee-hexo%E8%87%AA%E5%B7%B1%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2%E9%A1%B5%E9%9D%A2/), 将博客从 Gitee 迁移到 Github Pages 上, 并且还基于 Github 的 Issue 功能添加了评论功能, 希望有人能够多多评论交流, 也让我能找到一些共鸣. 

# 创建仓库

第一步是在 Github 上创建仓库, 这个仓库的名字构成如下: 

```shell
username.github.io
```

其中, `username` 就是 Github 上的用户名, Github 上的用户名是大小写敏感的. 我这里是 `Ann-Holmes`, 对应的仓库名为: `Ann-Holmes.github.io`. 

# 迁移博客

> [!Note]
> 现在博客的撰写和发布是在 Mac 上, 系统就是 MacOS. 因此, 下面的内容都是基于 MacOS 来说, 其他的系统我并未尝试, 后续如果有必要可能会尝试 Windows 系统和 Linux 系统. 

## 运行环境

三年前的运行环境虽然还在, 但是不太确定能不能跑起来, 就重新配置了一下运行环境. 

Hexo 可以通过 Node 进行安装, 因此需要先安装 Node. Mac 下可以通过 `Homebrew` 安装. 命令如下: 
```shell
brew install node 
```

之后可以用 `npm` 来全局安装 `hexo`, 具体参考 [Hexo 的文档](https://hexo.io/zh-cn/docs/#%E5%AE%89%E8%A3%85-Hexo), 我这里的安装命令如下: 
```shell
npm install -g hexo-cli
```

> [!Note]
> 这里是全局安装的, 方便后续使用 hexo 命令. 如果介意全局安装, 请参考 [Hexo 文档的进阶安装和使用](https://hexo.io/zh-cn/docs/#%E8%BF%9B%E9%98%B6%E5%AE%89%E8%A3%85%E5%92%8C%E4%BD%BF%E7%94%A8) 将 Hexo 以 `npm` 包的形式安装. 

安装完成之后, 就可以运行下面的命令来创建一个空白的 Hexo 站点: 
```shell
hexo init <folder>

e.g.
hexo init AnnBlog
```

Hexo 会创建一个名为 `AnnBlog` 的文件夹, 并且安装依赖项到这个文件夹下的 `node_modules` 中. 当然,  `AnnBlog` 还会包含一个 `.gitignore` 文件, 会将一些不必要的内容忽略掉. 

## 配置主题

幸运的是三年前选的主题, 现在还有人在维护, 非常感谢 [Icarus 主题](https://ppoffice.github.io/hexo-theme-icarus/about/)的维护者. 这里的主题也是可以作为依赖来安装的, 这种方式比较方便. 命令如下: 
```shell
# 进入刚才创建的文件夹
cd AnnBlog

# 安装 Icarus 主题
npm install hexo-theme-icarus
hexo config theme icarus
```

之后就是修改配置的内容, 我参考着三年前的配置内容和 [Icarus 的配置文档](https://ppoffice.github.io/hexo-theme-icarus/Configuration/icarus%E7%94%A8%E6%88%B7%E6%8C%87%E5%8D%97-%E4%B8%BB%E9%A2%98%E9%85%8D%E7%BD%AE/)进行了配置. 具体的内容, 这里就不赘述了, 如果有需要详细的配置文件, 可以去博客的源码仓库查看 [`_config.icarus.yml`](https://github.com/Ann-Holmes/Ann-Holmes.github.io/blob/main/_config.icarus.yml) 文件内容.  

## Hexo 配置

Hexo 配置也是参考三年前的配置和 [Hexo 的配置文档](https://hexo.io/zh-cn/docs/configuration)进行了配置, 同样可以去博客源码仓库查看 [`_config.yml`](https://github.com/Ann-Holmes/Ann-Holmes.github.io/blob/main/_config.yml).

# 评论系统

评论系统使用 Github Issues 来实现, Utterances Github App 可以帮助实现这个功能. Utterances 可以无缝地将 GitHub Issues 用于评论功能, 且与 GitHub Pages 协作流畅. 通过 Utterances 评论会自动作为 Issues 出现在 GitHub 仓库中, 之后就可以管理评论, 就像管理 Issues 一样: 回复、编辑、关闭或删除不当评论. 

使用 Icarus 主题并且集成 GitHub Issues 评论系统的配置步骤如下. 

## 安装 Utterances GitHub App

1. 访问 [Utterances GitHub App](https://github.com/apps/utterances) 页面。
2. 选择“Install”并为其配置访问博客存储库的权限。

## 配置 Utterances

在 [`_config.icarus.yml`](https://github.com/Ann-Holmes/Ann-Holmes.github.io/blob/main/_config.icarus.yml) 中找到评论(comments)的设置部分, 并且添加如下内容: 

```yaml
   comments:
     type: utterances
     repo: Ann-Holmes/Ann-Holmes.github.io  # 替换为你的 GitHub 用户名和仓库名
     issue_term: pathname
     theme: github-light  # 选择 'github-light' 或 'github-dark'
```

其中, repo 要替换成之前创建的用于存放个人博客的仓库, 这里我的是 `Ann-Holmes/Ann-Holmes.github.io`

# Github Actions 自动部署

三年前在 [Gitee ➕ Hexo 的记录内容](https://ann-holmes.github.io/2021/02/03/Gitee-hexo%E8%87%AA%E5%B7%B1%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2%E9%A1%B5%E9%9D%A2/) 中使用的是一键部署的方式来进行部署的. 这种部署方式在 Gitee Pages 上也不太方便, 而且源文件也不会同步到仓库中. 这次打算使用 Github Actions 来进行自动部署. 每次 push 之后, 就会自动进行部署服务. 如果 push 有更新内容, Github Actions 也会自动更新博客的内容. 

这里的配置也是参考 Hexo 的 Github Actions 部署来做的, 需要注意的一点是 Node 的版本, 其他的配置不用更改, 整体非常简单. 

首先, 在博客仓库中(以 `Ann-Holmes.github.io` 为例) 创建 `.github/workflows/pages.yml` 文件. 
这个文件就是 Github Action 用来自动部署 Pages 的配置文件. 

然后, 在 [`.github/workflows/pages.yml`](https://github.com/Ann-Holmes/Ann-Holmes.github.io/blob/main/.github/workflows/pages.yml) 中, 填写如下内容: 

```yaml
name: Pages

on:
  push:
    branches:
      - main  # default branch

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          # If your repository depends on submodule, please see: https://github.com/actions/checkout
          submodules: recursive
      - name: Use Node.js 21
        uses: actions/setup-node@v4
        with:
          # Examples: 20, 18.19, >=16.20.2, lts/Iron, lts/Hydrogen, *, latest, current, node
          # Ref: https://github.com/actions/setup-node#supported-version-syntax
          node-version: '21'
      - name: Cache NPM dependencies
        uses: actions/cache@v4
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache
      - name: Install Dependencies
        run: npm install
      - name: Build
        run: npm run build
      - name: Upload Pages artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public
  deploy:
    needs: build
    permissions:
      pages: write
      id-token: write
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

我这里的 node 版本为 21.7.3, 配置文件中 node 的版本就是 21, 配置文件中出现的 21 都是 node 的版本, 如果要修改都需要修改.  

# 结语

完成上面的所有配置之后, `git push -u origin main` 将本地仓库的修改 push 到 Github 上之后, 等上几分钟, 自动部署完成之后, 就可以访问博客了. 
希望这次搭建的博客不会半途而废, 坚持写博客提升自己的视野和技术, 也欢迎有缘人来一同探讨. 

虽然一个人可以走的很快, 但一群人可以走的更远. 
