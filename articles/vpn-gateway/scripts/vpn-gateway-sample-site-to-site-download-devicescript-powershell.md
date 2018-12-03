---
title: Azure PowerShell 脚本示例 - 下载设备配置模板 | Microsoft Docs
description: 下载设备配置模板。
services: vpn-gateway
documentationcenter: vpn-gateway
author: cherylmc
manager: jpconnock
editor: ''
tags: ''
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
origin.date: 04/30/2018
ms.date: 06/12/2018
ms.author: v-junlch
ms.openlocfilehash: d0894b20087027d5ea5be080d196da8be9d9fc4c
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52661311"
---
# <a name="download-vpn-device-template-using-powershell"></a>使用 PowerShell 下载 VPN 设备模板

此脚本下载用于连接的 VPN 设备模板


```azurepowershell
# Declare variables
$RG          = "TestRG1"
$GWName      = "VNet1GW"
$Connection  = "VNet1toSite1"

# List the available VPN device models and versions
Get-AzureRmVirtualNetworkGatewaySupportedVpnDevice -Name $GWName -ResourceGroupName $RG

# Download the configuration template for the connection
Get-AzureRmVirtualNetworkGatewayConnectionVpnDeviceConfigScript -Name $Connection -ResourceGroupName $RG `
-DeviceVendor Juniper -DeviceFamily Juniper_SRX_GA -FirmwareVersion Juniper_SRX_12.x_GA
```

## <a name="clean-up-resources"></a>清理资源

如果不再需要所创建的资源，请使用 [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) 命令删除资源组。 这将删除资源组及其包含的所有资源。

```azurepowershell
Remove-AzureRmResourceGroup -Name TestRG1
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建部署。 表中的每一项均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [Get-AzureRmVirtualNetworkGatewaySupportedVpnDevice](https://docs.microsoft.com/powershell/module/azurerm.network/Get-AzureRmVirtualNetworkGatewaySupportedVpnDevice) | 列出所有可用的 VPN 设备模型和版本。 |
| [Get-AzureRmVirtualNetworkGatewayConnectionVpnDeviceConfigScript](https://docs.microsoft.com/powershell/module/azurerm.network/Get-AzureRmVirtualNetworkGatewayConnectionVpnDeviceConfigScript) | 下载用于连接的配置模板。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

