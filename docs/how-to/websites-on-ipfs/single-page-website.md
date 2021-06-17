---
title: 在IPFS中托管单页面网站
legacyUrl: https://docs.ipfs.io/guides/examples/websites/
description: Learn how to host a simple one-page website on IPFS and link up a domain name.
---

# 在IPFS中托管单页面网站

本教程中，我们将在IPFS中托管一个简单的单页面网站，并将其关联到一个域名。这是一系列教程中的第一步，用于面向网站开发者说明如何在IPFS中构建网站和应用。

## 安装IPFS Desktop

IPFS desktop应用是快速启动和运行IPFS的最简单方法，其安装过程因系统而异，按以下相应系统的说明进行操作。

| [Windows](#windows)                                                        | [macOS](#macos)                                                      | [Linux](#linux)                                                      |
| -------------------------------------------------------------------------- | -------------------------------------------------------------------- | -------------------------------------------------------------------- |
| [![Windows icon](./images/single-page-website/windows-icon.png)](#windows) | [![macOS icon](./images/single-page-website/apple-icon.png)](#macos) | [![Linux icon](./images/single-page-website/linux-icon.png)](#linux) |

### Windows

1. 从[IPFS desktop下载页面](https://github.com/ipfs-shipyard/ipfs-desktop/releases)中下载最新可用的`.exe`文件：

   ![The IPFS desktop download page.](./images/single-page-website/install-windows-download-exe-page.png)

2. 运行`.exe`文件，以启动应用。
3. 选择是仅为自己安装还是为所有用户安装，然后点击**Next**：

   ![The IPFS desktop install options window.](./images/single-page-website/install-windows-install-options.png)

4. 选择应用程序的安装位置，默认位置即可，然后点击**Next**：

   ![The IPFS desktop installation location window.](./images/single-page-website/install-windows-install-location.png)

5. 等待安装完成，然后点击**Finish**：

   ![The IPFS desktop installation finished window.](./images/single-page-website/install-windows-install-finish.png)

6. 应该可以在系统状态栏中看到IPFS图标：

   ![The IPFS desktop status bar menu in the Windows status bar.](./images/single-page-website/install-windows-ipfs-desktop-status-bar.png)

此时IPFS Desktop应用已经安装完成，现在可以开始[添加站点](#add-your-site)。

### MacOS

1. 从[IPFS desktop downloads page](https://github.com/ipfs-shipyard/ipfs-desktop/releases)中下载最新可用的`.dmg`文件：

   ![List of available download links in GitHub.](./images/single-page-website/install-macos-dmg-file-link.png)

2. 打开`ipfs-desktop.dmg`文件。
3. 将IPFS图标拖入**Applications**文件夹中：

   ![Drag-to-install window in MacOS.](./images/single-page-website/install-macos-drag-ipfs-drag.png)

4. 打开**Applications**文件夹，然后打开IPFS desktop应用。
5. 如果出现警告：_IPFS Desktop.app can't be opened_，点击**Show in Finder**:

   ![Application cannot be installed error.](./images/single-page-website/install-macos-ipfs-cannot-be-opened.png)

6. 在**Applications** 文件夹中找到**IPFS Desktop.app**。
7. 按住`control`键，点击**IPFS Desktop.app**，然后点击**Open**：

   ![Right click context menu of IPFS Desktop.app.](./images/single-page-website/install-macos-force-open.png)

8. 在新窗口中点击**Open**：

   ![Open confirmation window.](./images/single-page-website/install-macos-open-confirmation.png)

9. 在系统状态栏中应该可以看到IPFS图标：

   ![The IPFS desktop status bar menu in the macOS status bar.](./images/single-page-website/install-macos-ipfs-desktop-status-bar.png)

此时IPFS Desktop应用已经安装完成，现在可以开始[添加站点](#add-your-site)。

### Linux

1. 从[IPFS desktop downloads page](https://github.com/ipfs-shipyard/ipfs-desktop/releases)下载最新可用的`.deb`文件：
1. 在**Software Installer**中打开`.deb`包：

   ![Right-click context menu of the IPFS deb package.](./images/single-page-website/install-ubuntu-software-install.png)

1. 点击**Install**，然后等待安装完成：

   ![Install screen within the Ubuntu software installation window.](./images/single-page-website/install-ubuntu-install.png)

1. 单击**Applications**，或者按下键盘的Windows键。
1. 搜索关键字`IPFS`，然后选择**IPFS Desktop**：

   ![Ubuntu search screen with IPFS desktop showing.](./images/single-page-website/install-ubuntu-search-window.png)

1. 在系统状态栏应该可以看到IPFS图标：

   ![IPFS icon shown in the Ubuntu status bar.](./images/single-page-website/install-ubuntu-ipfs-running-status-bar.png)

此时IPFS Desktop应用已经安装完成，现在可以开始[添加站点](#add-your-site)。

## 添加站点

下一步是在刚安装的IPFS Desktop中，将你的站点导入到IPFS中。这里示例的网站极其简单，主要用于随机展示与行星相关的知识。每次页面刷新的时候，都会显示一条新的知识。网站名我们称之为：_Random Planet Facts_。

1. 创建一个文件，名为`index.html`，然后粘贴以下代码到文件中：

   ```html
   <!DOCTYPE html>
   <html lang="en">
     <head>
       <meta charset="utf-8" />
       <title>Random Planet Facts</title>
       <meta
         name="description"
         content="Get a random fact about a planet in our solar system."
       />
       <meta name="author" content="The IPFS Docs team." />
       <style>
         body {
           margin: 15px auto;
           max-width: 650px;
           line-height: 1.2;
           font-family: sans-serif;
           font-size: 2em;
           color: #fff;
           background: #444;
         }
       </style>
     </head>
     <body onload="main()">
       <h1>Random Planet Facts</h1>
       <p id="output_p"></p>
       <script>
         function main() {
           const facts = [
             'Mars is home to the tallest mountain in our solar system.',
             'Only 18 out of 40 missions to Mars have been successful.',
             'Pieces of Mars have fallen to Earth.',
             'One year on Mars is 687 Earth days.',
             'The temperature on Mars ranges from -153 to 20 °C.',
             'One year on Mercury is about 88 Earth days.',
             'The surface temperature of Mercury ranges from -173 to 427°C.',
             'Mercury was first discovered in 14th century by Assyrian astronomers.',
             'Your weight on Mercury would be 38% of your weight on Earth.',
             'A day on the surface of Mercury lasts 176 Earth days.',
             'The surface temperature of Venus is about 462 °C.',
             'It takes Venus 225 days to orbit the sun.',
             'Venus was first discovered by 17th century Babylonian astronomers.',
             'Venus is nearly as big as the Earth with a diameter of 12,104 km.',
             "The Earth's rotation is gradually slowing.",
             'There is only one natural satellite of the planet Earth, the moon.',
             'Earth is the only planet in our solar system not named after a god.',
             'The Earth is the densest planet in the solar system.',
             'A year on Jupiter lasts around 4333 earth days.',
             'The surface temperature of Jupiter is around -108°C.',
             'Jupiter was first discovered by 7th or 8th century Babylonian astronomers.',
             'Jupiter has 4 rings.',
             'A day on Jupiter lasts 9 hours and 55 minutes.',
             'Saturn was first discovered by 8th century Assyrians.',
             'Saturn takes 10756 days to orbit the Sun.',
             'Saturn can be seen with the naked eye.',
             'Saturn is the flattest planet.',
             'Saturn is made mostly of hydrogen.',
             'Four spacecraft have visited Saturn.',
             'Uranus was discovered by William Herschel in 1781.',
             'A year on Uranus takes 30687 earth days.',
             'Uranus turns on its axis once every 17 hours, 14 minutes.',
             'With minimum atmospheric temperature of -224°C Uranus is nearly coldest planet in the solar system.',
             'Only one spacecraft has flown by Uranus, the Voyager 2.',
             'Neptune was discovered in 1846 by Urbain Le Verrier and Johann Galle.',
             'Neptune has 14 moons.',
             'The average temperatue of Neptune is about -201 °C.',
             'There is a 1:20 million scale model of the solar system in Sweden.',
             'The gap between the Earth and our moon is bigger than the diameters of all the planets combined.',
             "The first accurate calculation of the speed of light was using Jupiter's moons",
             "Jupiter's magnetic field is believed to be a result of rapidly spinning metallic hydrogen at the core, and is ~10x stronger than the Earth's.",
             'Venus spins backwards.',
             'Uranus spins sideways, relative to the ecliptic plane of the solar system.',
             'It is easier to reach Pluto or escape the solar system from Earth than being able to <i>land</i> on the Sun.'
           ]
           document.querySelector('#output_p').innerHTML =
             facts[Math.floor(Math.random() * facts.length)]
         }
       </script>
     </body>
   </html>
   ```

2. 打开IPFS desktop，然后进入**文件**页面。
3. 点击**导入** → **文件**.
4. 找到`index.html`文件，然后选择**打开**.

   ![Choose a file window open in IPFS desktop.](./images/single-page-website/add-ipfs-desktop-open-file.png)

5. 点击`index.html`右侧的`...`菜单，然后选择**分享链接**。
6. 点击**复制**将该文件的URL复制到剪贴板。

   ![Share files window in IPFS desktop.](./images/single-page-website/add-ipfs-desktop-share-files.png)

7. 打开浏览器，然后粘贴刚才复制的URL。

稍等一段时间后，浏览器应该加载显示此网站！第一次打开的时候可能会需要等待几分钟，页面加载时可以继续下一步。

## 固定文件

IPFS节点将他们存储的数据视为缓存，这意味着无法保证数据会一直保存着。 _固定_ 一个文件将通知IPFS节点此数据是重要的，不要丢弃它。对于重要的数据应该都 _固定_ 下来，以确保数据会长期保留。IPFS Desktop支持从 _文件_ 页面直接固定文件。

![IPFS Desktop application showing the pinning option.](./images/single-page-website/ipfs-desktop-showing-pinning.png)

然后，如果要确保本地IPFS节点离线时，IPFS数据仍然可访问，就需要使用其他选项，如 _协作集群_ 或者 _固定服务_。

### 协作集群

IPFS协作集群是由一组IPFS节点组成，这些节点相互协作来固定由一个或多个信任的节点添加的数据内容。你可以在此了解更多关于协作集群，包括如何自行配置集群的信息[cluster.ipfs.io](https://cluster.ipfs.io/documentation/collaborative/setup/)

### 固定服务

使用固定服务是确保重要数据被持续保留的最简单方法。该服务中包含大量的IPFS节点，可以为你固定住数据！采用这种方法，你不需要维护自己的IPFS节点。查看[持久化页面](/concepts/persistence)以获取固定服务的更多信息。在本教程中，我们将使用[Pinata](https://pinata.cloud/)，因为它为新用户提供了1GB的免费存储空间，而且有着非常简单的接口：

1. 访问[Pinata.cloud](https://pinata.cloud/)，然后注册或者登录。
2. 点击**Pinata Upload**.
3. 选择**Upload File**，然后点击**Browse**.
4. 找到`index.html`文件，然后点击**Open**.
5. 点击**Upload**.
6. 一旦文件上传成功，点击**Pin Explorer**以查看你固定的文件。
7. 应该可以看到`index.html`文件已经被固定了：

   ![The Pinata Pin Explorer screen showing the index.html pinned.](./images/single-page-website/pinned-index-file-in-pinata.png)

8. 点击`index.html`文件的**IPFS Hash**，以从Pinata网关打开你的网站。

   ![Random planet fact website pinned using Pinata and displayed in Firefox](./images/single-page-website/pinned-random-planet-fact-website.png)

## 配置域名

本节内容是完全可选的。

如果你能够访问像Namecheap、Google Domains、GoDaddy或其他的域名服务，那么可以继续后续步骤。如果还没有可分配的域名，可以只是了解下本章节。我们会在后续章节深入探讨如何使用去中心化名字服务，如Ethereum Naming Service (ENS)。

这里我们使用Namecheap来示例，但是这个流程在不同域名服务中都是类似的。

1. 登录到域名服务提供商的网站中。
2. 进入域名管理窗口，找到你想要分配给此网站的域名。
3. 找到修改 **重定向设置（Redirection Settings）** 的位置。
4. 在新窗口中，打开[Pinata Pin Explorer](https://pinata.cloud/pinexplorer)页面。
5. 复制**IPFS Hash**链接。
6. 在域名服务商的**Redirection Settings**项中，粘贴刚才复制的**IPFS Hash**的链接。

   ![Redirecting a source URL to an IPFS Hash link within Namecheap.](./images/single-page-website/namecheap-source-url-redirect.png)

7. 保存你的变更。

域名服务需要相当一段时间来更新信息，你可以在几小时后访问你的域名，来确认你固定的网站是否已经可以访问。

![Random planet facts site with the randomplanetfacts.xyz URL.](./images/single-page-website/random-planets-with-correct-url.png)

## 下一步

本项目是为了让你能够快速启动和运行，但我们还可以做更多的改进。

你应该发现，当访问[randomplanetfacts.xyz](http://randomplanetfacts.xyz)的时候，浏览器重定向到了[gateway.pinata.cloud/ipfs/QmW7S5HR...](https://gateway.pinata.cloud/ipfs/QmW7S5HRLkP4XtPNyT1vQSjP3eRdtZaVtF6FAPvUfduMjA)。这不是最佳的用户体验，还可能带来安全证书和其他网站验证方法的问题。同时，本网站极其简单，没有图片、外部样式表或javascript脚本。如果你想使用IPFS构建一个更复杂的网站，并确保其安全性，继续查看本教程之[在IPFS中托管多页面网站](/how-to/websites-on-ipfs/multipage-website)
