+++
title = "Ubuntu开箱"
author = ["Jackie Zhang"]
date = 2019-01-18T22:17:00+08:00
lastmod = 2019-02-28T23:37:12+08:00
tags = ["ubuntu", "setup"]
categories = ["misc"]
draft = false
weight = 3001
+++

记录下新装 Ubuntu 18.04 的环境配置。

<!--more-->


## sudo 免密码 {#sudo-免密码}

```sh
sudo su -
visudo
```

找到 root ALL=(ALL) ALL, 在其下面追加如下配置，即执行所有超级用户命令密码。

```sh
your_user_name ALL=(ALL) NOPASSWD: ALL
```

有的时候你的将用户设了 nopasswd，但是不起作用，原因是被后面的 group 的设置覆盖了，需要把 group 的设置也改为 nopasswd。

```sh
%admin ALL=(ALL) NOPASSWD: ALL
```


## 更新 {#更新}

```sh
sudo apt update && sudo apt upgrade -y
```


## 开启ssh远程登录 {#开启ssh远程登录}

```sh
sudo apt install -y openssh-server
```


## 安装基本工具 {#安装基本工具}

```sh
sudo apt install -y vim tmux supervisor build-essential pkg-config
```


## bash设置优化 {#bash设置优化}

```sh
echo 'set match-hidden-files off' >> ~/.inputrc
echo 'set show-all-if-ambiguous on' >> ~/.inputrc
echo 'set completion-ignore-case on' >> ~/.inputrc
echo '"\e[A": history-search-backward' >> ~/.inputrc
echo '"\e[B": history-search-forward' >> ~/.inputrc
```


## cuda {#cuda}

1.禁用nouveau

-   如果下面的指令有输出，表示nouveau驱动被加载，需手动禁用

```sh
lsmod | grep nouveau
```

-   执行

```sh
echo 'blacklist nouveau' | sudo tee -a /etc/modprobe.d/blacklist-nouveau.conf
echo 'options nouveau modeset=0' | sudo tee -a /etc/modprobe.d/blacklist-nouveau.conf
sudo update-initramfs -u
```

2.重启,不要进入图形界面

```sh
sudo reboot
```

3.运行官网下载的runfile


## cudnn {#cudnn}

下载对应cuda版本的cudnn，并解压

```sh
sudo cp -P include/cudnn.h /usr/local/cuda/include
sudo cp -P lib64/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
```
