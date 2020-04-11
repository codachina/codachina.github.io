---
title: 入门
toc: true
---

原文：https://codaprotocol.com/docs/

翻译：Star.LI (star@trapdoortech.com)

欢迎

欢迎来到[Coda Genesis计划](https://codaprotocol.com/genesis)的申请者！  测试网的第三阶段测试网已启动！ 我们将在[Coda Discord服务器](https://bit.ly/CodaDiscord)上的#announcements频道以及通过电子邮件发布更多公告。

**注册获取抵押代币**-[在此处注册](http://bit.ly/StakingSignup)，以接收抵押代币，加入测试网。 区块生产者（拥有Coda并参与共识的节点）是Coda网络的组成部分，而生产区块将帮助您赢得测试网挑战并积累测试网积分。 如果您对在测试网上注册有任何疑问，请与Discord服务器联系。

本节说明在本地计算机上运行Coda协议节点并连接到网络所需的要求。

注意

本文档适用于**beta**版本。 初始发行版之前，命令和API可能会更改。 最新的版本是“ 0.0.12-beta”。

## 软硬件要求

**软件**: macOS 或者 Linux （目前支持 Debian 9 和 Ubuntu 18.04 LTS）

注意：目前不支持Windows。 但是，社区成员成功在Windows系统上运行Linux，并运行节点。 单击[此处](https://forums.codaprotocol.com/t/unofficial-wsl-instructions/26)，获取社区创建的Windows节点的使用说明。

**硬件**：发送和接收Coda不需要任何特殊硬件，但是在Coda网络上运行区块生产节点目前需要：

- 最低4核CPU
- 最少8G内存

注意的是：如果你打算运行snark worker节点，则需要更多的内存 - 推荐使用16GB。GPU目前并不要求，但有可能后面协议升级需要。

**网络**: 最少1 Mbps带宽

**虚拟机实例**：O(1)Labs已在多个云主机平台上测试了运行节点，并为基本的节点需求推荐了以下实例。 请记住，自定义要求以及不同的成本约束可能需要不同的实例类型。

- AWS - [c5.2xlarge](https://www.ec2instances.info/?filter=c5.2xl&region=us-west-2&cost_duration=daily&selected=c5.2xlarge)
- GCP - [c2-standard-4](https://cloud.google.com/compute/docs/machine-types)
- Azure - [Standard_F8s_v2](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes-compute#fsv2-series-1)
- Digital Ocean - [c-8-16gib](https://cloud.digitalocean.com/droplets/new?size=c-8-16gib)

## 安装

提供了针对macOS和Linux的安装说明。可执行文件比较大，大约1GB，因此安装可能需要一些时间。

警告

如果您安装了老的版本`coda`，则需要对其进行升级，以免因使用较旧的客户端而连接不上网络的问题。 请参阅以下说明升级macOS和Linux可执行程序。

## macOS

使用 [Homebrew](https://brew.sh/)安装

```
brew install codaprotocol/coda/coda
```

如果您已经安装了旧版本，请运行：

```
brew upgrade coda
```

您可以运行 `coda -help` 确认安装是否成功。

## Ubuntu 18.04 / Debian 9

添加Coda Debian源，并安装：

```
sudo apt-get remove coda-testnet-postake-medium-curves
sudo apt-get remove coda-kademlia
echo "deb [trusted=yes] http://packages.o1test.net release main" | sudo tee /etc/apt/sources.list.d/coda.list
sudo apt-get update
sudo apt-get install -t release coda-testnet-postake-medium-curves
```

如果您安装了老的版本，通过上述命令，可以自动删除老版本并安装新版本。如果你第一次运行第一个命令，你会发现错误，E: Unable to locate package coda-testnet-postake-medium-curves`。请忽略该错误，该错误只是说明没有安装过Coda执行程序。

您可以运行 `coda -help` 确认安装是否成功。

## Windows

目前不支持Windows系统。如果你对支持Windows感兴趣，请联系support@o1labs.org或者通过[Discord server](https://bit.ly/CodaDiscord)沟通。

## 从源代码编译

如果你的系统是不同的Linux发行版本，或者不同的Mac OS版本，请查看 [try building Coda from source code](https://github.com/CodaProtocol/coda/blob/master/README-dev.md#building-coda)，从源代码编译可执行程序。注意的是，其他操作系统并没有充分测试，有可能存在问题。如遇问题，请通过Discord分享和寻求帮助。

## 设置端口和防火墙

如果您正在运行防火墙，则应允许8303端口的TCP数据通讯。此外，除非提供了“ -external-ip YOUR_IP”选项，否则守护程序将使用HTTPS（443）和HTTP（80）尝试确定自己的IP地址。

您可能需要配置路由器的端口转发，以允许通过您的**外部** IP地址到以下端口的入站流量。

- `TCP` 端口 `8302` 和 `8303`

其他说明，请查看 [this guide](https://codaprotocol.com/docs/troubleshooting/#port-forwarding)。

## 下一节

目前，你已经安装好Coda并设置好了网络，接下来是更有趣的事情 - [发送交易](https://codaprotocol.com/docs/my-first-transaction/)！

