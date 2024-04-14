---
title: Linux 服务器配置
toc: true
date: 2021-02-16 15:16:50
tags:
- Linux
- 知识碎片
categories:
- [知识碎片,Linux]
---

> 前言:
>
> 前一段时间趁双十一买了一个腾讯云的服务器, 现在有时间了, 来简单配置一下, 记录配置过程, 方便之后配置新的服务器. 

<!--more-->

# 拿到服务器之后

拿到服务器, 装好系统之后, 第一步我是先全部更新一遍软件: 

```shell
sudo apt update 
sudo apt upgrade
```

之后, 生成一套 `ssh` 秘钥, 以备不时之需:

```shell
ssh-keygen
# 一直回车即可, 不用填写内容
```

再然后, 将自己本机的 ssh 公钥放到服务对应的 `authorized_keys` 之中, 之后将服务器设置成只允许使用秘钥登录, 防止暴力破解. 

> ## 设置成只允许使用秘钥登录的方法: 
>
> ```shell
> sudo vim /etc/ssh/sshd_config
> ```
>
> 编辑 `/etc/ssh/sshd_config` 文件, 找到 `PasswordAuthentication yes` 这一行并修改 yes 为 no. 然后重启服务即可: 
>
> ```shell
> systemctl restart sshd.service
> ```

# 切换到 `zsh`:

```shell
sudo chsh $USER -s /bin/zsh
```

退出, 重新登录, 选择 `2` 使用最流行的 `.zhsrc` 文件启动 `zsh`. 

# 安装 `oh-my-zsh` 和 `zsh` 插件: 

```shell
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
# zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

# zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

```

> 注: 
>
> 1. 该步骤略微有点慢, 毕竟是从 GitHub 上安装的;
> 2. 只有这两个插件不是 `zsh` 自带的. 
>
> 3. 或者从本地复制对应的文件夹到服务器端: 
>
>    ```shell
>    tar -czf omz.tar.gz .oh-my-zsh/
>    scp omz.tar.gz tencentCloud:$SERVER_HOME/
>    cd $SERVER_HOME
>    tar -xzf omz.tar.gz
>    ```

# 配置 `zsh`

将本地的 `.zshrc` 文件复制到服务器下的家目录下即可: 

```shell
scp $HOME/.zshrc server@ip:$SERVER_HOME/.zshrc
```

# 配置 `vim` 

配置 `vim` 至可用的程度, 方便写一写简单的 `shell` 脚本: 

```shell
# 直接将本地的 vimrc 文件复制到服务器对应的目录下即可
scp $HOME/.vim/vimrc server@ip:$SERVER_HOME/.vim/
```

# 安装必备软件

## APT 安装

```shell
sudo apt install tree
sudo apt install trash-cli
# trash-restore 在 Ubuntu 上是 restore-trash

```

## miniconda 安装

```shell
# 下载最新版本的 miniconda
wget -c https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/Miniconda3-py39_4.9.2-Linux-x86_64.sh
# 安装
bash -c Miniconda*.sh
# 一路回车即可
# 激活 base 环境后安装 mamba
conda install -c conda-forge mamba
```

