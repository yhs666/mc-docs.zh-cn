---
title: "验证 VPN 网关连接 | Azure"
description: "本文介绍如何验证虚拟网络 VPN 网关连接。"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 7e3d1043-caa9-4472-96d3-832f4e2c91ee
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 04/24/2017
ms.date: 05/31/2017
ms.author: v-dazen
ms.openlocfilehash: aeff8da39b1ccd075ceb38290d8c001a49fd5578
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# 验证 VPN 网关连接
<a id="verify-a-vpn-gateway-connection" class="xliff"></a>

本文介绍如何验证 Resource Manager 和经典部署模型的 VPN 网关连接。

## 使用 Azure 门户验证
<a id="verify-using-the-azure-portal" class="xliff"></a>

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## 使用 PowerShell 验证
<a id="verify-using-powershell" class="xliff"></a>

若要使用 PowerShell 进行验证，请安装最新版本的 Azure Resource Manager PowerShell cmdlet。 有关安装 PowerShell cmdlet 的信息，请参阅[如何安装和配置 Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)。 有关使用 Resource Manager cmdlet 的详细信息，请参阅[将 Windows PowerShell 与 Resource Manager 配合使用](../powershell-azure-resource-manager.md)。

### 登录到 Azure 帐户
<a id="log-in-to-your-azure-account" class="xliff"></a>
1. 使用提升的权限打开 PowerShell 控制台，然后连接到帐户。

  ```powershell
  Login-AzureRmAccount -EnvironmentName AzureChinaCloud
  ```
2. 检查该帐户的订阅。

  ```powershell
  Get-AzureRmSubscription
  ``` 
3. 指定要使用的订阅。

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```

### 验证连接
<a id="verify-your-connection" class="xliff"></a>

[!INCLUDE [PowerShell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## 使用 Azure CLI 进行验证
<a id="verify-using-the-azure-cli" class="xliff"></a>

若要使用 Azure CLI 进行验证，请安装最新版本的 CLI 命令（2.0 或更高版本）。 有关安装 CLI 命令的信息，请参阅 [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)（安装 Azure CLI 2.0）。

### 登录到 Azure 帐户
<a id="log-in-to-your-azure-account" class="xliff"></a>

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

1. 使用 [az login](https://docs.microsoft.com/cli/azure/#login) 命令登录到 Azure 订阅，并按照屏幕上的说明进行操作。

  ```azurecli
  az login
  ```
2. 如果有多个 Azure 订阅，请列出该帐户的订阅。

  ```azurecli
  Az account list --all
  ```
3. 指定要使用的订阅。

  ```azurecli
  Az account set --subscription
  <replace_with_your_subscription_id>
  ```

### 验证连接
<a id="verify-your-connection" class="xliff"></a>

[!INCLUDE [CLI](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]

## 使用 Azure 门户（经典）验证
<a id="verify-using-the-azure-portal-classic" class="xliff"></a>
[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## 使用 PowerShell 验证（经典）
<a id="verify-using-powershell-classic" class="xliff"></a>
若要使用 PowerShell 进行验证，请安装最新版本的 Azure PowerShell cmdlet。 请务必同时下载并安装 Resource Manager 和服务管理 (SM) 版本。 有关安装 PowerShell cmdlet 的信息，请参阅[如何安装和配置 Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)。 

### 登录到 Azure 帐户
<a id="log-in-to-your-azure-account" class="xliff"></a>
1. 使用提升的权限打开 PowerShell 控制台，然后连接到帐户。

  ```powershell
  Login-AzureRmAccount -EnvironmentName AzureChinaCloud
  ```
2. 检查该帐户的订阅。

  ```powershell
  Get-AzureRmSubscription 
  ```
3. 指定要使用的订阅。

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```
4. 登录以便使用经典部署模型的 Service Management cmdlet。

  ```powershell
  Add-AzureAccount -Environment AzureChinaCloud
  ```

### 验证连接
<a id="verify-your-connection" class="xliff"></a>
[!INCLUDE [Classic PowerShell](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

## 后续步骤
<a id="next-steps" class="xliff"></a>
* 可以将虚拟机添加到虚拟网络。 请参阅[创建虚拟机](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)以获取相关步骤。
