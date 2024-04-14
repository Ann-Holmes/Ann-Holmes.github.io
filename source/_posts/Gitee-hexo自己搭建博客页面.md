---
title: Gitee+hexo 搭建一个属于自己的博客平台
toc: true
date: 2021-02-03 17:11:38
tags: 
- hexo 
- 知识碎片
categories: 
- [知识碎片,hexo]
---

# 准备工作——应该知道的背景

从上个与开始需要每两周给老板汇报一次工作，在这个过程中获益匪浅，终于明白了写总结的重要性，于是心里萌生自己建一个博客的想法。有想法就要立刻行动起来。之前也略微听过说 Github 和 Gitee 都提供网页服务，但是具体的还不清楚。通过简单的查阅选择了 Hexo 和 Gitee 搭配来搭建自己的博客。

<!--more-->

## Gitee

Gitee 是开源中国旗下的一个代码托管平台，与 Github 的服务类似，但是仓库数量上远不及 Github，只是在国内的访问速度比 Github 快很多，于是选为博客的网页服务平台。

## hexo

> Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 [Markdown](http://daringfireball.net/projects/markdown/)（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

# 初始化——让博客跑起来

我认为搭建博客平台的首要目标应该是让博客可以正常运行，之后再搞那些花里胡哨的东西。那话不多说，我们先通过简单几步让博客运行起来。

## 安装 hexo

安装按照官网提供的说明自己安装就好，目前常用的平台是 mac 平台。安装步骤如下：

1. 安装 `node.js`，安装代码如下：

```shell
brew install node
```

> 注意：我这里是用 [Homebrew](https://brew.sh/) 安装的

2. 安装 hexo

```shell
npm install -g hexo-cli
```

> 作为新手为了避免出错，建议完整安装

3. 使用 hexo 建站

```shell
hexo init myblog
# 进入到 myblog 目录
cd myblog
# 启动 hexo 本地服务
hexo s
```

启动 hexo 服务后, 访问 http://localhost:4000 查看页面如果可以显示出 Hello, world 页面, 表示 hexo 安装成功, 并且 hexo 也成功跑起来了. 

{% asset_img image-20210203181917103.png "hexo初始页面'hexo初始页面" %}

## 配置 Gitee

上面只是在本地跑了起来, 如果需要在网络上也能访问, 需要有一个服务器来提供后台服务, 租服务器是不可能的, 白嫖才是最快乐的. Gitee 刚好提供了给 WordPress, hexo 等静态博客的网页服务. 下面就来配置 Gitee 让其提供这种服务.

> 注册好 Gitee 账号之后, 新建一个仓库, 仓库名和自己的用户名一致. 一定要记得勾上"使用 README 文件初始化仓库", 不然该仓库是没有 网页服务的. 

具体的注册方法和配置方法参考[该博客](https://blog.csdn.net/cungudafa/article/details/104260494)中的第三部分. 

关键配置是将 Gitee 上的仓库的链接放到 `_config.yml` 中的 deploy 部分. 

```yml
deploy:
  type: git
  repo: git@gitee.com:name/name.git 
  # 由于我已经在本地电脑上配置了 ssh-key 所以就采用 ssh 推送的方式, 省去了输入密码的步骤
  branch: master 
```

配置好之后, 通过下面代码就可以一键将本地的服务部署到 Gitee 上了

```bash
hexo clean && hexo g && hexo d
```

运行完上述代码之后, hexo 会根据 `source/_posts` 文件夹下的 markdown 文件生成对应的静态网页, 并做一次 `git commit` 同时将生成的静态文件推送到 Gitee 远程仓库中. 所以本地的 hexo 的源文件是没有推送到远程 Gitee 仓库的. 

> Gitee 上不提供自动更新, 所以每次都要手动更新, 后面如果有时间再查一查有没有一键部署的工具, 可以同时把 Gitee 也更新. 

## 源码备份仓库*

> 该部分是可选的. 

如上所说, hexo 本地的源文件是没有推送到远程仓库的, 于是我又另外建了一个 Gitee 仓库, 用来存放 hexo 服务的源码. 这个很简单, 只用将 `myblog` 文件夹下的所有文件拷贝到一个空的 Gitee 仓库, 后面就是用 Git 来管理每一次写的博客就可以了. 

# 更换主题——让你的博客属于你

为了让博客看起来更加符合自己的审美, 下一步我们就开始配置主题. 这里我选择的主题是 Icarus. 该主题的预览效果如下: 

![Icarus 效果图](https://img-blog.csdnimg.cn/20210203223809658.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1NDkwMzkz,size_16,color_FFFFFF,t_70#pic_center)

## hexo-theme-Icarus

> Icarus是静态网站生成器Hexo的一款简单，精致，而现代的主题。 它力求设计上的优雅，但也不抛弃使用上的简单明了。 它灵活且多功能的配置系统让资深用户也能极尽细节地装饰他们的站点。 Icarus同时也提供了超多插件与挂件来满足你的多元的站点个性化和优化需求。 除此以外，它的崭新实现使得更好的IDE支持和第三方接入成为可能，并提供了更多未来的优化空间。

## 安装 Icarus

安装只需要将 Icarus 的所有内容放到 `hexo` 的 `themes` 文件夹下即可. 我使用的是如下命令: 

```shell
git clone https://github.com/ppoffice/hexo-theme-icarus.git themes/icarus --depth 1
```

## 基本配置

Icarus 的官方文档写的很详细, 所以我是先按照官方文档的说明按照自己的需求修改了一遍配置之后, 再着重去查一些特殊的配置. 

> Icarus 的[官方文档](https://blog.zhangruipeng.me/hexo-theme-icarus/).

需要记录的几点特殊配置:

1. icon 多个外部链接的 icon 后面需要跟添加的 FontAwesome5 的字体, 这里需要去查看 FontAwesome5 的[官网](https://fontawesome.com/icons?d=gallery)去找对应的图标的标记.
2. 如果要生成文章的目录, 不仅需要在 `config` 中的 `toc` 部分修改为 `true`, 而且还需要在 markdown 文件开头的 `yml` 中引入 `toc: true` 这样文章才会生成目录, 我在 `scanfold` 文件中将 post.md 模板文件添加了 `toc: true`, 这样之后的文章就会自动生成目录了.
3. `mathjax` 要设置成 `true`, 这样页面才会根据 $\LaTeX$ 语法渲染公式. 
4. `provider` 部分要把后面两个改成 `koil` 来提供服务, 这样国内访问才会比较快. 

## 插件配置

### baidu_analytics

我只使用了 baidu_analytics 插件, 这个插件根据 Icarus 提供的官方文档配置即可使用. 

### 评论插件

目前 villia 在 Icarus 上的状态很奇怪, 暂时没有使用. 

# 图片管理——更舒适的使用体验

我目前认为图片管理最好还是建一个同名的文件夹, 然后使用如下语句添加图片: 

```shell
{% asset_img file_name "图注'alt'" %}
```

Icarus 提供在创建新文章的时候同时创建同名的文件夹作为存储图片, 只需要修改配置文件中的 `post_asset_folder` 为 `true` , 就可以在运行新增博客命令的时候, 自动生成同名文件夹: 

```shell
hexo new <file_name> 
```



当然, 如果使用图床会更好, 可以在 typora 中实时看到渲染的效果, 并及时修改, 但是上床图片到图床很不方便. 之前本以为可以用 CSDN 作为图床来使用, 但是 CSDN 的编辑器太难用了, 最后还是老老实实的使用同名文件夹的方法来进行图片管理. 

# 参考

1. https://www.zhihu.com/column/c_1262685442828414976

2. https://blog.csdn.net/cungudafa/article/details/104260494

