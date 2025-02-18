---
title: NAT配置
description: Fix common connection issues by managing your routers NAT and forwarding ports so other IPFS nodes can interact with your node.
---

# NAT配置

新手运行IPFS常见的一个问题，尤其是在家庭网络中，就是他们的IPFS节点位于NAT（网络地址转换）后面。对于在家庭网络中运行IPFS节点的人来说，等待时间很长或者请求失败率很高是常见的事情。这是因为IPFS网络中的其他节点很难跨越他们的NAT来连接到他们。NAT允许许多机器来共享一个公网地址，因此对于IPv4协议的可持续运行至关重要，否则它的32位地址空间将无法满足现代网络人群的需求。

当连接到家庭WiFi时，计算机会获得一个类似`10.0.1.15`形式的IPv4地址，这是保留给私有网络内部使用的保留地址范围内的地址。当你发起一个传出连接到公网地址时，路由器会把你的内部IP替换为它自身的公网IP地址，然后当数据从远端返回时，路由器又会将它转换为内部地址。

虽然NAT对传出连接通常都是透明的，监听传入连接则需要一些配置。路由器仅在一个公网IP地址上监听，但内部网络可以有无数的机器来处理请求。为了处理这些请求，路由器需要能将特定的流量发送到特定的机器上。可以按以下任意一种方式来配置路由器以支持此类连接：

## IPv6

如果你的路由器和互联网服务提供商（ISP）支持IPv6，启用它能够解决一些连接问题。大部分当前的路由器都有一个独立选项用于启用IPv6，这里我们无法给出每种路由器的具体设置选项。可以在你的路由器生产商的网站上搜索IPv6，以获悉如何启用它。

## UPnP

如果你的路由器支持UPnP，IPFS会尝试自动允许入站流量来访问你的本地内容。一些家用路由器可能需要配置以明确启用UPnP。这里我们无法给出每种路由器的具体设置选项。可以在你的路由器生产商的网站上搜索UPnP。

## Port forwarding

每种路由器都有不同的选项和方案来设置端口转发。大部分路由器厂商都会有一个教程来说明如何在他们的设备中设置端口转发。通常的步骤如下：

1. 登录到路由器页面中。
1. 找到路由器的端口转发的设置项。
1. 输入你的IPFS节点的IP地址。
1. 设置流量通过的端口为`4001`。
1. 重启你的IPFS节点以使更改生效。确认要重启整个机器，而不仅仅是IPFS守护进程。

假设你不打算将守护进程的API开放到公网中，那么只需要打开端口`4001`/`tcp`。
