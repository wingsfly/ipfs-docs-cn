---
title: 绑定域名
description: Allow users to access your IPFS hosted site by linking up a domain name.
---

# 绑定域名

用户可以通过在浏览器地址栏中输入内容ID（CID）来访问你的网站。然而，正如IP地址一样，CID对用户来说看起来并不易于辨识。为解决这个问题，我们可以把域名和CID映射关联起来，这样当用户访问`www.YourDomain.com`时，就可以访问你托管在IPFS上的站点。本教程说明了如何通过DNS来提供的常规域名，以及通过以太坊名字服务提供去中心化域名。

本章节完全可选，但可以提供如何管理域名服务和IPFS的信息。

## 域名服务(DNS)

以下将展示如何将域名和CID映射关联起来。这和具体的域名服务注册商无关，但是具体的链接和设置操作位置可能不太一样。

### 事先准备

开始教程前，我们需要：

- 一个域名，最好是尚未注册到特定网站的域名。
- 托管在IPFS中网站的CID。如果已经按之前教程操作，当前应该已经拥有了一个对应网站的CID。

1. 访问域名注册商的控制面板，找到配置当前域名`CNAME`和`TXT`记录的位置。
1. 创建一个`CNAME`记录。
   a. 设置**Host**为`@`。
   b. 设置**Value**为`gateway.ipfs.io.`。注意在`gateway.ipfs.io.`最后还有一个点`.`。
1. 创建一个`TXT`记录。
   a. 设置**Host**为`_dnslink`。
   b.设置值为`dnslink=/ipfs/SITE_CID`，将`SITE_CID`替换为你网站所对应的CID。
1. 保存你的修改。

DNS更新需要一段时间以传播到互联网中，然后你的域名应该最终指向了你的IPFS托管站点！现在可以尝试在[以太坊名字服务](#ethereum-naming-service)中进行同样的设置。

## 以太坊名字服务(ENS)

以太坊名字服务(ENS)是一个定位资源的去中心化方案。正如DNS将人类可读的名称转为IP地址，ENS将人类可读的名称如`randomplanetfacts.eth.link`转为以太坊地址。这些地址可用于指向IPFS中的CID资源。这里不涉及细节，ENS旨在解决DNS的一些问题，主要是中间人攻击和可扩展性。更多关于DNS的问题，可以查看[Cynthia Taylor在recompilermag.com上的报告](https://recompilermag.com/issues/issue-1/the-web-is-broken-how-dns-breaks-almost-every-design-principle-of-the-internet/).

### 事先准备

需要事先准备好以下内容，以使用ENS的域名服务：

- 已安装[Metamask](https://metamask.io/)浏览器插件。
- 一个已有一些`ETH`的以太坊账号。
- 一个托管在IPFS中的网站。如果已经按之前教程操作，当前应该已经具备一个网站和对应的CID。
- 一个已想好的很不错的域名！

:::tip 域名价格
域名的价格取决于以下信息：

- 想要购买的域名。
- 当前ETH的价格。
- 此交易所对应的gas费用。
- 准备持有域名的时长。

选择预付的年份越长，在gas上累积所花费的金额就越少：`(1 year + 1 gas fee) < (10 years + 1 gas fee) < (10 * (1 year + 1 gas fee))`
:::

### 购买以太坊域名

1. 访问[app.ens.domains](https://app.ens.domains/)。
2. 登陆MetaMask：

   ![Metamask pop-up showing a login screen.](./images/link-a-domain/ens-metamask-log-into-key.png)

3. 查询你想要的域名：

   ![Searching for a domain in ENS.](./images/link-a-domain/ens-search-for-domain-name.png)

4. 如果域名可用，点击此域名。
5. 点击**Request To Register**:

   ![Registration screen within ENS.](./images/link-a-domain/ens-request-to-register.png)

6. 在弹出的MetaMask窗口中，点击**Confirm**。这个操作将会扣除`ETH`。
7. 等待 _Request to register_ 交易完成，可能需要若干分钟：

   ![Registration screen within ENS.](./images/link-a-domain/ens-registration-transaction-pending.png)

8. ENS在交易完成后，还需要等待约1分钟，以确认同一时刻没有其他人也在购买相同的域名：

   ![Waiting for registration confirmation screen in ENS.](./images/link-a-domain/ens-wait-a-minute.png)

9. 点击**Register**，然后在弹出的MetaMask窗口中点击**Confirm**：

   ![2nd transaction confirmation within MetaMask.](./images/link-a-domain/ens-metamask-complete-registration-transaction.png)

10. 等待交易被确认，这个需要若干分钟：

    ![Waiting for the 2nd transaction to be verified.](./images/link-a-domain/ens-complete-registration.png)

    现在应该可以看到`.eth`域名的所有设置：

    ![All the domain settings within ENS](./images/link-a-domain/ens-domain-settings-page.png)

### 绑定IPFS内容ID(CID)

11. 点击**Records**之后的`+`图标：

    ![The "add new records" icon in ENS.](./images/link-a-domain/ens-add-records-icon.png)

12. 在下拉菜单中选择**Content**：

    ![Records dropdown menu in ENS.](./images/link-a-domain/ens-add-content-record.png)

13. 在**Content**文本框中输入网站所对应的CID，前缀应该为`ipfs://`：

    ![Setting the content record as an IPFS CID.](./images/link-a-domain/ens-set-content-record-as-ipfs-cid.png)

14. 在MetaMask弹出窗口中点击**Confirm**，以确认更新：

    ![MetaMask confirmation pop-up for changing a content record.](./images/link-a-domain/ens-metamask-content-record-transaction.png)

    该交易需要等待若干分钟以完成。

若干分钟后，你应该可以打开`Your_Domain.eth/`以访问你的网站，注意尾部要带一个斜杠`/`。因为`.eth`不是一个已注册的顶级域名，它通常无法通过常规的浏览器来访问。

[Eth.link](https://eth.link/)提供了一种方式，使得任意浏览器可以访问你的网站。在你的域名`Your_Domain.eth.link`后面追加一个`.link`，然后就可以正常访问你的网站了。

## 下一步

此系列的下一个教程，会去了解一个工具，用于简化整个操作过程：[Fleek](/how-to/websites-on-ipfs/introducing-fleek)。
