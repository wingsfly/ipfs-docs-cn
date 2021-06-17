---
title: IPFS更新器
description: The IPFS updater is a command-line tool originally used to help users update their IPFS version. Learn how to install, upgrade, and downgrade Go-IPFS using the IPFS updater.
---

# IPFS更新器

IPFS更新器是一个命令行工具，用于帮助用户更新IPFS版本。它当前已经升级为还允许用户安装Go-IPFS。安装IPFS更新器最简单的方式是使用预构建的二进制文件，详情如下。如果选择从源代码构建，查看[项目仓库]](https://github.com/ipfs/ipfs-update#from-source)。

## 安装更新器

可以从 [`dist.ipfs.io`](https://dist.ipfs.io/#ipfs-update)下载预构建的二进制文件，二进制文件还可以从[IPFS Update GitHub release page](https://github.com/ipfs/ipfs-update/releases)获取。

### Windows

1. 从[`dist.ipfs.io`](https://dist.ipfs.io/#ipfs-update)下载Windows文件。

   ```powershell
   cd ~
   wget https://dist.ipfs.io/ipfs-update/v1.6.0/ipfs-update_v1.6.0_windows-amd64.zip -Outfile ipfs-update_v1.6.0_windows-amd64.zip
   ```

2. 解压到合适的位置：

   ```powershell
   Expand-Archive -Path ipfs-update_v1.6.0_windows-amd64.zip -DestinationPath ~\Apps\ipfs-update_v1.6.0
   ```

3. 进入`ipfs-update_v1.6.0`目录，确认`ipfs-update.exe`可运行：

   ```powershell
   cd Apps\ipfs-update_v1.6.0\ipfs-update\
   .\ipfs-update.exe --version

   > ipfs-update version 1.6.0
   ```

   现在`ipfs-update`已经可用了，建议按以下步骤，把`ipfs-update.exe`添加到`PATH`中。

4. 显示当前工作目录，并复制到剪贴板中：

   ```powershell
   pwd

   > Path
   > ----
   > C:\Users\Johnny\Apps\ipfs-update_v1.6.0\ipfs-update
   ```

5. 检查PowerShell配置文件是否存在：

   ```powershell
   Test-Path $profile

   > false
   ```

   如果配置文件已存在，跳过以下步骤直接转到第七步。

6. 创建PowerShell配置文件：

   ```powershell
   New-Item -path $profile -type file –force

   > Mode                LastWriteTime         Length Name
   > ----                -------------         ------ ----
   > -a----        11/5/2020   6:38 PM              0 Microsoft.PowerShell_profile.ps1
   ```

7. 在位于`Documents\WindowsPowerShell`的`Microsoft.PowerShell_profile.ps1`文件中，将复制的目录添加到PowerShell的`PATH`中：

   ```powershell
   Add-Content C:\Users\Johnny\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1 "[System.Environment]::SetEnvironmentVariable('PATH',`$Env:PATH+';;C:\Users\Johnny\Apps\ipfs-update_v1.6.0\ipfs-update')"
   ```

8. 关闭再重新打开PowerShell。进入用户根目录，查询`ipfs-update`版本信息，以确认`PATH`已设置正确：

   ```powershell
   cd ~
   ipfs-update --version

   > ipfs-update version 1.6.0
   ```

   如果再次打开PowerShell后，在加载配置文件时提示错误，需要更改PowerShell的`ExecutionPolicy`值为`Unrestricted`，具体参见[Microsoft PowerShell documentation](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-7)中的描述。

### macOS

1. 从[`dist.ipfs.io`](https://dist.ipfs.io/#ipfs-update)下载macOS文件。

   ```bash
   curl https://dist.ipfs.io/ipfs-update/v1.6.0/ipfs-update_v1.6.0_darwin-amd64.tar.gz --output ipfs-update_v1.6.0_darwin-amd64.tar.gz
   ```

2. 解压文件：

   ```bash
   tar -xvzf ipfs-update_v1.6.0_darwin-amd64.tar.gz

   > x ipfs-update/install.sh
   > x ipfs-update/ipfs-update
   ```

3. 进入`ipfs-update`目录，运行安装脚本：

   ```bash
   cd ipfs-update
   sudo bash install.sh

   > installed /usr/local/bin/ipfs-update
   ```

4. 确认`ipfs-update`成功安装：

   ```bash
   ipfs-update --version

   > ipfs-update version 1.6.0
   ```

### Linux

1. 从[`dist.ipfs.io`](https://dist.ipfs.io/#ipfs-update)下载Linux文件：

   ```bash
   wget https://dist.ipfs.io/ipfs-update/v1.6.0/ipfs-update_v1.6.0_linux-amd64.tar.gz
   ```

2. 解压文件：

   ```bash
   tar -xvzf ipfs-update_v1.6.0_linux-amd64.tar.gz

   > x ipfs-update/install.sh
   > x ipfs-update/ipfs-update
   ```

3. 进入`ipfs-update`目录，运行安装脚本：

   ```bash
   cd ipfs-update
   sudo bash install.sh

   > installed /usr/local/bin/ipfs-update
   ```

4. 确认`ipfs-update`成功安装：

   ```bash
   ipfs-update --version

   > ipfs-update version 1.6.0
   ```

## 安装IPFS

运行`ipfs-update install`，后边跟着需要安装的Go-IPFS版本号：

```bash
ipfs-update install 0.5.0
```

要安装最新的Go-IPFS发行版本，使用`latest`标签：

```bash
ipfs-update install latest
```

`ipfs-update install`会下载、验证并安装特定版本的Go-IPFS。如果已安装另一版本的IPFS，该版本会被隐藏起来，以备后续可以恢复。

## 降级IPFS

使用`revert`功能来回滚至之前的Go-IPFS版本：

```bash
ipfs-update revert
```

`ipfs-update revert`会还原至之前的Go-IPFS安装版本。此功能可用于在最新安装的版本出现问题，需要切换为原有稳定版本的时候。

## 卸载更新器

需要卸载IPFS更新器时，删除对应的二进制文件，并从`PATH`中移除`ipfs-update`。

### Windows

1. 获取到`ipfs-update.exe`文件的路径：

   ```powershell
   gci -recurse -filter ipfs-update.exe -File -ErrorAction SilentlyContinue

   > Directory: C:\Users\Johnny\Apps\ipfs-update_v1.6.0\ipfs-update
   ```

2. 删除`ipfs-update`目录：

   ```powershell
   Remove-Item -Recurse -Force C:\Users\Johnny\Apps\ipfs-update_v1.6.0
   ```

3. 从`PATH`中移除`ipfs-update`目录，该操作因Windows安装而异，详情参见[Microsoft documentation for details](https://docs.microsoft.com/en-us/cpp/build/setting-the-path-and-environment-variables-for-command-line-builds?view=msvc-160)。

### Linux & macOS

1. 查询`ipfs-update`文件的路径：

   ```bash
   sudo find / -name ipfs-update

   /usr/local/bin/ipfs-update
   ```

2. 移除该文件：

   ```bash
   sudo rm /usr/local/bin/ipfs-update
   ```
