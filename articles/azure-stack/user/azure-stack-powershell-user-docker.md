---
title: 使用 Docker 运行适用于 Azure Stack 的 PowerShell | Microsoft Docs
description: 使用 Docker 在 Azure Stack 上运行 PowerShell
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: article
origin.date: 04/25/2019
ms.date: 06/03/2019
ms.author: v-jay
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: c87ec1f7e1b700ed9d7602638b5731815b9d9136
ms.sourcegitcommit: 77d6ceb6a14a3316a6088859c4d9978115b2454a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/27/2019
ms.locfileid: "66249141"
---
# <a name="use-docker-to-run-powershell"></a>使用 Docker 运行 PowerShell

可以使用 Docker 创建基于 Windows 的容器，在其上运行特定版本的 PowerShell，该版本是使用不同的界面所需要的。 必须在 Docker 中使用基于 Windows 的容器。

## <a name="docker-prerequisites"></a>Docker 先决条件

1. 安装 [Docker](https://docs.docker.com/install/)。
2. 打开命令行（例如 Powershell 或 Bash），然后键入：

    ```bash
        Docker -version
    ```

3. 需要使用需要 Windows 10 的 Windows 容器来运行 Docker。 在运行 Docker 时，请切换到 Windows 容器。

4. 必须从已加入 Azure Stack 所在的域的计算机运行 Docker。 如果使用 ASDK，需[在远程计算机上安装 VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn)。

## <a name="service-principals-for-using-powershell"></a>适合使用 PowerShell 的服务主体

需要在 Azure AD 租户中有一个服务主体，以便使用 PowerShell 访问 Azure Stack 中的资源。 通过基于角色的访问控制来委派权限。

1. 按照[通过创建服务主体向应用程序授予对 Azure Stack 资源的访问权限](azure-stack-create-service-principals.md)一文中的说明设置主体。
2. 记下应用程序 ID、机密和租户 ID。

## <a name="docker---azure-stack-api-profiles-module"></a>Docker - Azure Stack API 配置文件模块

Dockerfile 打开 Microsoft 映像 microsoft/windowsservercore，其中已安装 Windows PowerShell 5.1。 该文件然后会加载 NuGet 和 Azure Stack PowerShell 模块，并从 Azure Stack Tools 下载工具。

1. 将此存储库下载为 Zip 文件，或者克隆存储库：  
[https://github.com/mattbriggs/azure-stack-powershell](https://github.com/mattbriggs/azure-stack-powershell)

2. 从终端打开存储库文件夹。

3. 在存储库中打开命令行界面并键入：

    ```bash  
    docker build --tag azure-stack-powershell .
    ```

4. 生成映像以后，可以启动交互式容器。 键入：

    ```bash  
        docker run -it azure-stack-powershell powershell
    ```

5. 可以将此 shell 用于 cmdlet 了。

    ```bash
    Windows PowerShell
    Copyright (C) 2016 Microsoft Corporation. All rights reserved.

    PS C:\>
    ```

6. 使用服务主体连接到 Azure Stack。 现在使用 Docker 中的 PowerShell 提示符。 

    ```Powershell
    $passwd = ConvertTo-SecureString <Secret> -AsPlainText -Force
    $pscredential = New-Object System.Management.Automation.PSCredential('<ApplicationID>', $passwd)
    Connect-AzureRmAccount -ServicePrincipal -Credential $pscredential -TenantId <TenantID>
    ```

   PowerShell 返回帐户对象：

    ```PowerShell  
    Account    SubscriptionName    TenantId    Environment
    -------    ----------------    --------    -----------
    <AccountID>    <SubName>       <TenantID>  AzureCloud
    ```

7. 通过在 Azure Stack 中创建资源组，测试连接性。

    ```PowerShell  
    New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
    ```

## <a name="next-steps"></a>后续步骤

-  阅读 [Azure Stack 上的 Azure Stack PowerShell](azure-stack-powershell-overview.md) 概述。
- 了解 Azure Stack 上的 [PowerShell 的 API 配置文件](azure-stack-version-profiles.md)。
- [安装 Azure Stack Powershell](../operator/azure-stack-powershell-install.md)。
- 了解如何创建 [Azure 资源管理器模板](azure-stack-develop-templates.md)以实现云一致性。