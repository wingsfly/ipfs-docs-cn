---
title: 命令行
description: Using IPFS through the command-line allows you to do everything that IPFS Desktop can do, but at a more granular level since you can specify which commands to run. Learn how to install it here.
---

# Command-line

如果准备在IPFS节点之上构建应用程序或服务，通过命令行安装IPFS是一个很方便的办法。当你通过非图形界面安装节点时该方法同样有效，常见于在远程服务器或虚拟机上安装。通过命令行使用IPFS可以访问IPFS桌面的所有功能，同时因为可以指定运行的命令，操作可以更加精细。

![A terminal window running the IPFS daemon in Ubuntu.](./images/command-line/terminal-showing-ipfs-daemon-ubuntu.png)

## 系统需求

IPFS需要至少512MiB内存，可以在树莓派中运行IPFS节点。然而，IPFS安装所需要的磁盘空间取决于期望共享的数据大小。基础安装需要大约12MB磁盘空间，同时[默认最大磁盘使用量](/how-to/configure-node)设为10GB。

## 官方发行版

IPFS团队提供了[dist.ipfs.io网站](https://dist.ipfs.io/)以帮助用户快速获取IPFS各种平台最新的版本。一旦IPFS包的新版本发布，它就自动发布在`dist.ipfs.io`，以确保用户可以获取到最新版本。以下步骤显示了如何使用命令行从`dist.ipfs.io`下载和安装Go-IPFS 0.8.0。

| [Windows](#windows)                                                          | [macOS](#macos)                                                        | [Linux](#linux)                                                        |
| ---------------------------------------------------------------------------- | ---------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| [![Windows icon](./images/command-line/windows-icon.png =250x200)](#windows) | [![macOS icon](./images/command-line/apple-icon.png =250x200)](#macos) | [![Linux icon](./images/command-line/linux-icon.png =250x200)](#linux) |

### Windows

1. 从[`dist.ipfs.io`](https://dist.ipfs.io/#go-ipfs)下载windows二进制文件。

   ```powershell
   cd ~\
   wget https://dist.ipfs.io/go-ipfs/v0.8.0/go-ipfs_v0.8.0_windows-amd64.zip -Outfile go-ipfs_v0.8.0.zip
   ```

2. 解压该文件到合适的位置。

   ```powershell
   Expand-Archive -Path go-ipfs_v0.8.0.zip -DestinationPath ~\Apps\go-ipfs_v0.8.0
   ```

3. 进入`go-ipfs_v0.8.0`目录，并确认`ipfs.exe`可运行：

   ```powershell
   cd ~\Apps\go-ipfs_v0.8.0\go-ipfs
   .\ipfs.exe --version

   > ipfs version 0.8.0
   ```

   此时已经可以使用IPFS了，建议按以下步骤将`ipfs.exe`添加到`PATH`中。

4. 显示当前的工作目录，并复制到剪贴板中：

   ```powershell
   pwd

   > Path
   > ----
   > C:\Users\Johnny\Apps\go-ipfs_v0.8.0\go-ipfs
   ```

5. 在位于`Documents\WindowsPowerShell`中的`profile.ps1`文件尾部追加以下内容，以将刚才复制的目录添加到PowerShell's `PATH`中：

   ```powershell
   Add-Content C:\Users\Johnny\Documents\WindowsPowerShell\profile.ps1 "[System.Environment]::SetEnvironmentVariable('PATH',`$Env:PATH+';;C:\Users\Johnny\Apps\go-ipfs_v0.8.0\go-ipfs')"
   ```

6. 关闭并重新打开PowerShell，再进入用户根目录，查询IPFS版本，以确认IPFS路径设置生效：

   ```powershell
   cd ~
   ipfs --version

   > ipfs version 0.8.0
   ```

### macOS

1. 从[`dist.ipfs.io`](https://dist.ipfs.io/#go-ipfs)下载macOS二进制文件。

   ```bash
   wget https://dist.ipfs.io/go-ipfs/v0.8.0/go-ipfs_v0.8.0_darwin-amd64.tar.gz
   ```

1. 解压文件：

   ```bash
   tar -xvzf go-ipfs_v0.8.0_darwin-amd64.tar.gz

   > x go-ipfs/install.sh
   > x go-ipfs/ipfs
   > x go-ipfs/LICENSE
   > x go-ipfs/LICENSE-APACHE
   > x go-ipfs/LICENSE-MIT
   > x go-ipfs/README.md
   ```

1. 进入`go-ipfs`目录，执行安装脚本：

   ```bash
   cd go-ipfs
   bash install.sh

   > Moved ./ipfs to /usr/local/bin
   ```

1. 确认IPFS已正确安装：

   ```bash
   ipfs --version

   > ipfs version 0.8.0
   ```

### Linux

1. 从[`dist.ipfs.io`](https://dist.ipfs.io/#go-ipfs)下载Linux二进制文件。

   ```bash
   wget https://dist.ipfs.io/go-ipfs/v0.8.0/go-ipfs_v0.8.0_linux-amd64.tar.gz
   ```

1. 解压文件：

   ```bash
   tar -xvzf go-ipfs_v0.8.0_linux-amd64.tar.gz

   > x go-ipfs/install.sh
   > x go-ipfs/ipfs
   > x go-ipfs/LICENSE
   > x go-ipfs/LICENSE-APACHE
   > x go-ipfs/LICENSE-MIT
   > x go-ipfs/README.md
   ```

1. 进入`go-ipfs`目录，运行安装脚本：

   ```bash
   cd go-ipfs
   sudo bash install.sh

   > Moved ./ipfs to /usr/local/bin
   ```

1. 确认IPFS已正确安装：

   ```bash
   ipfs --version

   > ipfs version 0.8.0
   ```

## 手动编译

手动编译IPFS是一个复杂的过程，且经常发生变化。它只适用于需要构建一个特定分支版本或者使用Go-IPFS前沿版本的时候，查看[`ipfs/go-ipfs` GitHub仓库 →](https://github.com/ipfs/go-ipfs)以获取详情

## 下一步

当前已经完成了IPFS的安装，可以在此网络之上开始构建应用程序和服务了！查看命令行快速使用指南，直接访问[初始化仓库部分](http://docs.ipfs.io/how-to/command-line-quick-start/#initialize-the-repository).
