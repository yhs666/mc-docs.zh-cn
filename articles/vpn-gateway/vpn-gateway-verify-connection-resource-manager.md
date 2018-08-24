---
title: 验证 VPN 网关连接 | Microsoft 文档
description: 本文介绍如何验证虚拟网络 VPN 网关连接。
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: 7e3d1043-caa9-4472-96d3-832f4e2c91ee
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 05/16/2017
ms.date: 08/13/2018
ms.author: v-junlch
ms.openlocfilehash: 711b331112066a836c87c1ae940a46d26bf2993b
ms.sourcegitcommit: ec76e0b9a869eb833f76f6bebbabd2d61ed17e0c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2018
ms.locfileid: "41705248"
---
# <a name="verify-a-vpn-gateway-connection"></a>验证 VPN 网关连接

本文介绍如何验证经典部署模型和 Resource Manager 部署模型的 VPN 网关连接。

## <a name="azure-portal"></a>Azure 门户

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="powershell"></a>PowerShell

若要使用 PowerShell 验证 Resource Manager 部署模型的 VPN 网关连接，请安装最新版本的 [Azure Resource Manager PowerShell cmdlet](https://docs.microsoft.com/powershell/azure/overview)。

[!INCLUDE [PowerShell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="azure-cli"></a>Azure CLI

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

若要使用 Azure CLI 验证 Resource Manager 部署模型的 VPN 网关连接，请安装最新版本的 [CLI 命令](/cli/install-azure-cli)（2.0 或更高版本）。

[!INCLUDE [CLI](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]

## <a name="azure-portal-classic"></a>Azure 门户（经典）

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## <a name="powershell-classic"></a>PowerShell（经典）

若要使用 PowerShell 验证经典部署模型的 VPN 网关连接，请安装最新版本的 Azure PowerShell cmdlet。 请务必下载并安装 [Service Management](https://docs.microsoft.com/powershell/azure/servicemanagement/install-azure-ps?view=azuresmps-4.0.0#azure-service-management-cmdlets) 模块。 使用“Add-AzureAccount”登录经典部署模型。

[!INCLUDE [Classic PowerShell](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

## <a name="next-steps"></a>后续步骤

* 可以将虚拟机添加到虚拟网络。 请参阅[创建虚拟机](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)以获取相关步骤。

<!-- Update_Description: link update -->