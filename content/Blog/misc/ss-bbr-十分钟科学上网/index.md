+++
title = "SS+BBR 十分钟科学上网"
author = ["Jackie Zhang"]
date = 2018-05-26T18:19:00+08:00
lastmod = 2019-03-02T11:17:01+08:00
tags = ["shadowsocks"]
categories = ["misc"]
draft = false
weight = 3002
+++

听闻不少小伙伴还在购买他人提供的VPN，这里分享下自己搭建梯子的方法。

<!--more-->

---

`2019-03-01 更新：`

-   最近GCP新加了香港机房，实测延迟较台湾略低。
-   鉴于非常时期的一些非常手段，建议加密方式选用 rc4-md5，端口往大了填。

---

以自己使用的GCP为例（300刀一年免费），也可以选用别家VPS。


## 一、配置VM {#一-配置vm}


### 1.创建VM {#1-dot-创建vm}

进入[GCP官网](https://cloud.google.com/)注册，此处需要一张VISA信用卡。
进入控制台后，在左侧菜单 -> Compute Engine -> VM实例，点击创建。地区选择台湾，系统选择 Debian 9 或 Ubuntu 18.04，其余配置按余额自由发挥，勾上允许HTTP和HTTPS流量，添加SSH密钥，创建。


### 2.配置静态IP {#2-dot-配置静态ip}

左侧菜单 -> VPC网络 -> 外部IP地址，将IP类型调整为静态。


### 3.配置防火墙规则 {#3-dot-配置防火墙规则}

创建防火墙规则，开放指定入站端口供shadowsocks使用，比如45678。


## 二、装梯子 {#二-装梯子}


### 1.远程连接 {#1-dot-远程连接}

在VM实例中，点击对应SSH，就可在浏览器中打开远程终端。
也可以在本地终端中，利用添加的SSH密钥远程登录，并更新系统。

```sh
ssh your-google-account@your-VM-IP

sudo su -
apt update
apt upgrade
```

注：更新过程中若出现google-cloud-SDK更新失败的错误，不要在意，是因为选择的VM配置太低造成的


### 2.开启BBR {#2-dot-开启bbr}

开启BBR拥塞控制算法，需要内核版本4.9以上，Debian 9 默认4.9，Ubuntu 18.04 默认4.15。

```sh
modprobe tcp_bbr
echo "tcp_bbr" >> /etc/modules-load.d/modules.conf
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl -p
```

重启后执行lsmod ，看到有tcp\_bbr模块即说明 bbr 已启动。


### 3.安装配置shadowsocks {#3-dot-安装配置shadowsocks}

```sh
apt install shadowsocks
```

查看ssserver参数。

```sh
ssserver -h
```

以守护进程形式运行。

```sh
ssserver -k your-password -p your-server-port -d start
```

最简单的只需配置密码和端口，端口为防火墙打开的端口，不输入默认8388要在防火墙中打开。
也可使用config.json文件配置启动参数。


## 三、架飞机 {#三-架飞机}

使用各系统版本的shadowsocks客户端或命令行，配置相应服务器设置，就可以起飞啦！
[Windows](https://github.com/shadowsocks/shadowsocks-windows/releases)
[Android](https://github.com/shadowsocks/shadowsocks-android/releases)
[MacOS](https://github.com/shadowsocks/ShadowsocksX-NG/releases)
[Linux](https://github.com/shadowsocks/shadowsocks-qt5/releases)
