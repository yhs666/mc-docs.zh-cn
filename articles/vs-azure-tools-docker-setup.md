---
title: "使用 VirtualBox 配置 Docker 主机 | Azure"
description: "使用 Docker Machine 和 VirtualBox 配置默认 Docker 实例的分步说明"
services: azure-container-service
documentationCenter: na
authors: allclark
manager: douge
editor: 
ms.service: multiple
ms.topic: article
origin.date: 06/08/2016
ms.date: 06/20/2016
ms.author: v-junlch
ms.openlocfilehash: 23544692a2ca5c00c278e99c82ad9f3c8ee2459a
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="configure-a-docker-host-with-virtualbox"></a>使用 VirtualBox 配置 Docker 主机

## <a name="overview"></a>概述
本文将引导你完成使用 Docker Machine 和 VirtualBox 配置默认 Docker 实例的过程。 如果使用的是 [Docker for Windows beta 版](http://beta.docker.com/)，则无需进行此配置。

## <a name="prerequisites"></a>先决条件
需要安装以下工具。

- [Docker 工具箱](https://www.docker.com/products/overview#/docker_toolbox)

## <a name="configuring-the-docker-client-with-windows-powershell"></a>使用 Windows PowerShell 配置 Docker 客户端

要配置 Docker 客户端，只需打开 Windows PowerShell，并执行以下步骤：

1. 创建默认 Docker 主机实例。

    ```
    docker-machine create --driver virtualbox default
    ```

1. 验证默认实例是否已配置且在运行。 （应该会看到名为“default”的实例正在运行。

    ```PowerShell
    docker-machine ls 
    ```

    ![][0]

1. 将“default”设置为当前主机，并配置 Shell。

    ```PowerShell
    docker-machine env default | Invoke-Expression
    ```

1. 显示活动的 Docker 容器。 列表应为空。

    ```PowerShell
    docker ps
    ```

    ![docker ps 的输出][1]

> [!NOTE]
> 每次重启开发计算机时，都需要重启本地 Docker 主机。
> 为此，请在命令提示符下发出以下命令：`docker-machine start default`。

[0]: ./media/vs-azure-tools-docker-setup/docker-machine-ls.png
[1]: ./media/vs-azure-tools-docker-setup/docker-ps.png