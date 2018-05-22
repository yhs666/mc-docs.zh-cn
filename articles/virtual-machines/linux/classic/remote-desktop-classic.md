---
title: 使用远程桌面连接到 Linux VM | Azure
description: 了解如何安装和配置远程桌面以连接到经典部署模型的 Azure Linux VM
services: virtual-machines-linux
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: 34348659-ddb7-41da-82d6-b5885859e7e4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
origin.date: 05/30/2017
ms.date: 05/21/2018
ms.author: v-yeche
ms.openlocfilehash: 0ebd7f6303da8524a1dd7271f6fb416f9a2c7274
ms.sourcegitcommit: c3084384ec9b4d313f4cf378632a27d1668d6a6d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2018
---
# <a name="using-remote-desktop-to-connect-to-a-azure-linux-vm"></a>使用远程桌面连接到 Azure Linux VM
> [!IMPORTANT] 
> Azure 提供两个不同的部署模型用于创建和处理资源：[Resource Manager 和经典模型](../../../resource-manager-deployment-model.md)。 本文介绍如何使用经典部署模型。 Azure 建议大多数新部署使用 Resource Manager 模型。 有关本文中的资源管理器的更新版本，请参阅[此文](../use-remote-desktop.md)。

## <a name="overview"></a>概述
RDP（远程桌面协议）是 Windows 使用的专用协议。 如何使用 RDP 远程连接到 Linux VM（虚拟机）？

此指南将为你提供答案！ 它将帮助你在 Azure Linux VM 上安装和配置 xrdp，使你能够从一台 Windows 计算机通过远程桌面与其连接。 本指南将使用运行 Ubuntu 或 OpenSUSE 的 Linux VM 作为示例。

Xrdp 工具是一个开源 RDP 服务器，支持从 Windows 计算机通过远程桌面连接到 Linux 服务器。 RDP 的性能比 VNC（虚拟网络计算）更好。 VNC 使用 JPEG 质量图形呈现，并且速度可能会很慢，而 RDP 则更加快速和清晰。

> [!NOTE]
> 必须已有运行 Linux 的 Microsoft Azure VM。 若要创建和设置 Linux VM，请参阅 [Azure Linux VM 教程](createportal-classic.md)。
> 
> 

## <a name="create-an-endpoint-for-remote-desktop"></a>为远程桌面创建终结点
在本文档中，我们将使用默认终结点 3389 进行远程连接。因此，将 Linux VM 的 3389 终结点设置为 `Remote Desktop`，如下所示：

![图像](./media/remote-desktop/endpoint-for-linux-server.png)

如果不知道如何设置 VM 终结点，请参阅[本指南](setup-endpoints.md)。

## <a name="install-gnome-desktop"></a>安装 Gnome 桌面
通过 `putty` 连接到 Linux VM，并安装 `Gnome Desktop`。

对于 Ubuntu，使用：

```bash
sudo apt-get update
sudo apt-get install ubuntu-desktop
```

对于 OpenSUSE，请使用：

```bash
sudo zypper install gnome-session
```

## <a name="install-xrdp"></a>安装 xrdp
对于 Ubuntu，使用：

```bash
sudo apt-get install xrdp
```

对于 OpenSUSE，请使用：

> [!NOTE]
> 使用你在以下命令中使用的版本更新 OpenSUSE 版本。 以下为 `OpenSUSE 13.2` 示例。
> 
> 

```bash
sudo zypper in http://download.opensuse.org/repositories/X11:/RemoteDesktop/openSUSE_13.2/x86_64/xrdp-0.9.0git.1401423964-2.1.x86_64.rpm
sudo zypper install tigervnc xorg-x11-Xvnc xterm remmina-plugin-vnc
```

## <a name="start-xrdp-and-set-xdrp-service-at-boot-up"></a>启动时启动 xrdp 并设置 xdrp 服务
对于 OpenSUSE，请使用：

```bash
sudo systemctl start xrdp
sudo systemctl enable xrdp
```

对于 Ubuntu，安装后，在启动时会自动启动并启用 xrdp。

## <a name="using-xfce-if-you-are-using-an-ubuntu-version-later-than-ubuntu-1204lts"></a>如果使用比 Ubuntu 12.04LTS 更高的 Ubuntu 版本，请使用 xfce
因为当前的 xrdp 版本不支持比 Ubuntu 12.04LTS 更高的 Ubuntu 版本的 Gnome 桌面，我们将使用 `xfce` 桌面。

请使用以下命令安装 `xfce`：

```bash
sudo apt-get install xubuntu-desktop
```

然后使用下面的命令启用 `xfce`：

```bash
echo xfce4-session >~/.xsession
```

编辑配置文件 `/etc/xrdp/startwm.sh`：

```bash
sudo vi /etc/xrdp/startwm.sh   
```

在 `/etc/X11/Xsession` 行之前添加 `xfce4-session` 行。

若要重启 xrdp 服务，请使用：

```bash
sudo service xrdp restart
```

## <a name="connect-your-linux-vm-from-a-windows-machine"></a>从 Windows 计算机连接 Linux VM
在 Windows 计算机中，启动远程桌面客户端，并输入 Linux VM DNS 名称。 或转到 Azure 门户中的 VM 仪表板并单击 `Connect` 连接 Linux VM。 在这种情况下，将显示登录窗口：

![图像](./media/remote-desktop/no2.png)

使用 Linux VM 的用户名和密码登录。

## <a name="next-steps"></a>后续步骤
有关使用 xrdp 的详细信息，请参阅 [http://www.xrdp.org/](http://www.xrdp.org/)。

<!-- Update_Description: update meta properties, update link -->
