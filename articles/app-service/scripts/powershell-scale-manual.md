---
title: Azure PowerShell 脚本示例 - 手动缩放 Web 应用 | Azure
description: Azure PowerShell 脚本示例 - 手动缩放 Web 应用
services: app-service\web
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: de5d4285-9c7d-4735-a695-288264047375
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
origin.date: 03/20/2017
ms.date: 01/21/019
ms.author: v-biyu
ms.custom: mvc
ms.openlocfilehash: e8afe9c734309641130daf891084abb7fcbbc72a
ms.sourcegitcommit: 90d5f59427ffa599e8ec005ef06e634e5e843d1e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2019
ms.locfileid: "54083933"
---
# <a name="scale-a-web-app-manually"></a>手动缩放 Web 应用

在此方案中，将了解如何创建资源组、应用服务计划和 Web 应用。 然后，将应用服务计划从单个实例扩展到多个实例。

必要时，请使用 [Azure PowerShell 指南](https://docs.microsoft.com/powershell/azure/overview)中的说明安装 Azure PowerShell，并运行 `Connect-AzureRmAccount -EnvironmentName AzureChinaCloud` 创建与 Azure 的连接。

## <a name="sample-script"></a>示例脚本

```powershell

# Generates a Random Value
$Random=(New-Guid).ToString().Substring(0,8)

# Variables
$ResourceGroupName="myResourceGroup$random"
$AppName="AppServiceManualScale$random"
$Location="ChinaNorth"

# Create a Resource Group
New-AzureRMResourceGroup -Name $ResourceGroupName -Location $Location

# Create an App Service Plan
New-AzureRMAppservicePlan -Name AppServiceManualScalePlan -ResourceGroupName $ResourceGroupName -Location $Location -Tier Basic

# Create a Web App in the App Service Plan
New-AzureRMWebApp -Name $AppName -ResourceGroupName $ResourceGroup -Location $Location -AppServicePlan AppServiceManualScalePlan

# Scale Web App to 2 Workers
Set-AzureRMAppServicePlan -NumberofWorkers 2 -Name AppServiceManualScalePlan -ResourceGroupName $ResourceGroupName
```

## <a name="clean-up-deployment"></a>清理部署 

运行脚本示例后，可以使用以下命令删除资源组、Web 应用以及所有相关资源。

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzureRmAppServicePlan](https://docs.microsoft.com/powershell/module/azurerm.websites/new-azurermappserviceplan) | 创建应用服务计划。 |
| [New-AzureRmWebApp](https://docs.microsoft.com/powershell/module/azurerm.websites/new-azurermwebapp) | 创建 Web 应用。 |
| [Set-AzureRmWebApp](https://docs.microsoft.com/powershell/module/azurerm.websites/set-azurermwebapp) | 修改 Web 应用的配置。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure PowerShell 示例](../samples-powershell.md)中找到 Azure 应用服务 Web 应用的其他 Azure Powershell 示例。
