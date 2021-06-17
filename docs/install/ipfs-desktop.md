---
title: IPFS Desktop
description: IPFS Desktop gives you all the power of IPFS in a convenient desktop app - a complete IPFS node, plus handy OS menu shortcuts and an all-in-one file manager, peer map, and content explorer.
---

# IPFS Desktop

**IPFS Desktop将IPFS节点、文件管理器、节点管理器和内容浏览器集成到了一个易于使用的应用中。**

通过IPFS Desktop可以无需操作控制台命令即可熟悉IPFS功能。或者如果你已经熟悉了IPFS命令，使用强大的菜单/工具栏快捷方式结合控制台命令，可以加速你的IPFS工作流程。

如果你的计算机中已经有了IPFS节点，IPFS Desktop将作为该节点的控制面板和文件浏览器角色而存在。如果之前没有IPFS节点，则IPFS桌面会安装一个节点。任何一种方式下，IPFS都会自动检查更新。

![IPFS Desktop的状态界面](./images/ipfs-desktop/desktop-status.png)

| 文件管理界面 | 内容浏览界面 | 节点界面 | 配置界面 | 任务栏菜单 |
| - | - | - | - | --- |
| ![Screenshot of the Files screen](./images/ipfs-desktop/desktop-files.png) | ![Screenshot of the Explore screen](./images/ipfs-desktop/desktop-explore.png) | ![Screenshot of the Peers screen](./images/ipfs-desktop/desktop-peers.png) | ![Screenshot of the Settings screen](./images/ipfs-desktop/desktop-settings.png) | ![Screenshot of Mac/Windows menus](./images/ipfs-desktop/desktop-menubar-taskbar.png) |

### 功能特性

- **开机自启动(Mac/Windows)，支持操作系统管理** using the convenient menubar/system tray menu
- **快速导入文件/文件夹/截屏到IPFS** in a variety of convenient ways, including drag-and-drop and (for Windows) right-clicking a file/folder's icon
- **便捷管理节点的内容** with a familiar file browser that offers quick shortcuts for renaming/moving/pinning files and folders, previewing many common file formats directly in IPFS Desktop, copying content IDs or shareable links to your clipboard, and more
- **快捷下载CID、IPFS路径和IPNS路径** — choose `Download...` from your menubar/status tray, paste in a hash, and you're good to go
- **IPFS全球节点可视化** on a map depicting what nodes you're connected to, where they are, the connections they're using, and more
- **浏览IFPS文件的"Merkle Forest"** with a visualizer that lets you see firsthand how example datasets stored on IPFS — or your own IPFS files — are broken down into content-addressed pieces
- **IPFS文件和链接的系统级支持** (on Mac, Windows, and some Linux flavors) automatically hands off links starting with `ipfs://`, `ipns://` and `dweb:` to be opened in IPFS Desktop
- **CLI教程模式** helps you learn IPFS commands as you go

### 安装说明

按照操作系统相对应的说明来安装IPFS Desktop。IPFS Desktop使用了[Electron framework](https://www.electronjs.org)，因此在Electron可用的环境下，该应用均可使用。

| [Windows](#windows)                                                 | [macOS](#macos)                                               | [Ubuntu](#Ubuntu)                                                |
| ------------------------------------------------------------------- | ------------------------------------------------------------- | ---------------------------------------------------------------- |
| [![Windows icon](./images/ipfs-desktop/windows-icon.png)](#windows) | [![macOS icon](./images/ipfs-desktop/apple-icon.png)](#macos) | [![Ubuntu icon](./images/ipfs-desktop/ubuntu-icon.png)](#ubuntu) |

如果你想使用包管理器来安装，查看IPFS社区维护的[第三方包列表](#package-managers)。

## Windows

1. 打开[IPFS Desktop下载页面](https://github.com/ipfs-shipyard/ipfs-desktop/releases).
2. 找到IPFS Desktop以`.exe`结尾的最新版本链接：

   ![IPFS Desktop下载页面](./images/ipfs-desktop/install-windows-download-exe-page.png)

3. 运行该`.exe`文件，安装应用。
4. 选择是只为自己安装还是为所有用户安装。点击**Next**:

   ![The IPFS Desktop install options window.](./images/ipfs-desktop/install-windows-install-options.png)

5. 选择程序安装路径。通常选择默认路径，然后点击**Next**:

   ![The IPFS Desktop installation location window.](./images/ipfs-desktop/install-windows-install-location.png)

6. 等待安装结束，并点击**Finish**:

   ![The IPFS Desktop installation finished window.](./images/ipfs-desktop/install-windows-install-finish.png)

7. 现在应当可以在桌面托盘的状态栏中看到IPFS图标：

   ![The IPFS Desktop status bar menu in the Windows status bar.](./images/ipfs-desktop/install-windows-ipfs-desktop-status-bar.png)

IPFS桌面安装完成，现在可以开始[添加站点](#add-your-site).

## macOS

1. 从[ipfs-shipyard/ipfs-desktop发布页面](https://github.com/ipfs/ipfs-desktop/releases)下载最新可用的`.dmg`文件:

   ![List of available download links in GitHub.](./images/ipfs-desktop/install-macos-dmg-file-link.png)

2. 打开`ipfs-desktop.dmg`文件。
3. 将IPFS图标拖入**Applications**文件夹：

   ![Drag-to-install window in MacOS.](./images/ipfs-desktop/install-macos-drag-ipfs-drag.png)

4. 打开**Applications**文件夹，然后打开IPFS桌面程序。
5. 如果提示警告：_IPFS Desktop.app can't be opened_，点击**Show in Finder**:

   ![An error message showing that the computer cannot install the application.](./images/ipfs-desktop/install-macos-ipfs-cannot-be-opened.png)

6. 在 **Applications**文件夹中找到**IPFS Desktop.app**。
7. 按住`control`键，然后点击**IPFS Desktop.app**，选择**Open**：

   ![Right click context menu of IPFS Desktop.app.](./images/ipfs-desktop/install-macos-force-open.png)

8. 在新窗口中点击**Open**：

   ![Open confirmation window.](./images/ipfs-desktop/install-macos-open-confirmation.png)

9. 然后在系统状态栏中应该可以看到IPFS图标：

   ![The IPFS Desktop status bar menu in the macOS status bar.](./images/ipfs-desktop/install-macos-ipfs-desktop-status-bar.png)

IPFS Desktop安装完成，现在可以开始[添加站点](#add-your-site).

## Ubuntu

此说明针对的是Ubuntu系统，但其他Ubuntu相关的发行版应当同样适用。对于非Ubuntu的Linux发行版，可以查看[IPFS Desktop GitHub repository](https://github.com/ipfs-shipyard/ipfs-desktop#install)中的安装说明。

1. 从[IPFS Desktop GitHub仓库](https://github.com/ipfs-shipyard/ipfs-desktop#install)下载最新的`.AppImage`包。

1. 进入下载`.AppImage`文件的目录，为文件添加可执行权限：

   ```shell
   cd Downloads
   chmod a+x /ipfs-desktop-linux.AppImage
   ```

1. 在控制台执行`./ipfs-desktop-linux.AppImage`以打开`.AppImage`：

   ```shell
   ./ipfs-desktop-linux.AppImage
   ```

   你也可以在文件浏览器中双击`.AppImage`文件以执行。

## 包管理器

| 包管理器                                                                                                    | 命令                      |
| ------------------------------------------------------------------------------------------------------------------ | ---------------------------- |
| [Homebrew](https://formulae.brew.sh/formula/ipfs#default)                                                                    | `brew install ipfs --cask`     |
| [Chocolatey](https://community.chocolatey.org/packages/ipfs)                                                         | `choco install ipfs-desktop` |
| [Scoop](https://github.com/lukesampson/scoop-extras/blob/master/bucket/ipfs-desktop.json) maintained by [@NatoBoram](https://github.com/NatoBoram) | `scoop bucket add extras && scoop install ipfs-desktop` |
| [AUR](https://aur.archlinux.org/packages/ipfs-desktop/) maintained by [@alexhenrie](https://github.com/alexhenrie) | `ipfs-desktop`               |

## 下一步

当前已经安装了IPFS Desktop，现在可以开始共享文件，并与其他网络中的节点进行交互了！查看如何[在IPFS中托管网站 →](/how-to/websites-on-ipfs/single-page-website/)

