---
title: Azure PowerShell 脚本示例 - 使用 Web 服务器日志监视 Web 应用 | Azure
description: Azure PowerShell 脚本示例 - 使用 Web 服务器日志监视 Web 应用
services: app-service\web
documentationcenter: ''
author: syntaxc4
manager: erikre
editor: ''
tags: azure-service-management
ms.assetid: 5805d7cd-9e56-4eba-bd85-75b013690ff5
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
origin.date: 03/20/2017
ms.date: 09/04/2019
ms.author: v-tawe
ms.custom: mvc
ms.openlocfilehash: 4388f537523c2515142930bf08e801b95a866dba
ms.sourcegitcommit: bc34f62e6eef906fb59734dcc780e662a4d2b0a2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2019
ms.locfileid: "70806696"
---
# <a name="monitor-a-web-appwith-web-server-logs"></a>使用 Web 服务器日志监视 Web 应用

在此方案中，将创建资源组、应用服务计划、Web 应用，并配置 Web 应用以启用 Web 服务器日志。 然后，下载日志文件以供查看。

必要时，请使用 [Azure PowerShell 指南](https://docs.microsoft.com/powershell/azure/overview)中的说明安装 Azure PowerShell，并运行 `Connect-AzAccount -Environment AzureChinaCloud` 创建与 Azure 的连接。

## <a name="sample-script"></a>示例脚本

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
```powershell
# Generates a Random Value
$Random=(New-Guid).ToString().Substring(0,8)

# Variables
$ResourceGroupName="myResourceGroup$Random"
$AppName="AppServiceMonitor$Random"
$Location="ChinaNorth"

# Create a Resource Group
New-AzResourceGroup -Name $ResourceGroupName -Location $Location

# Create an App Service Plan
New-AzAppservicePlan -Name AppServiceMonitorPlan -ResourceGroupName $ResourceGroupName -Location $Location -Tier Basic

# Create a Web App in the App Service Plan
New-AzWebApp -Name $AppName -ResourceGroupName $ResourceGroupName -Location $Location -AppServicePlan AppServiceMonitorPlan

# Enable Logs
Set-AzWebApp -RequestTracingEnabled $True -HttpLoggingEnabled $True -DetailedErrorLoggingEnabled $True -ResourceGroupName $ResourceGroupName -Name $AppName

# Make a Request
Invoke-WebRequest -Method "Get" -Uri https://$AppName.chinacloudsites.cn/404 -ErrorAction SilentlyContinue

# Download the Web App Logs
#Get-AzWebAppMetrics -ResourceGroupName $ResourceGroupName -Name $AppName -Metrics

```

## <a name="clean-up-deployment"></a>清理部署 

运行脚本示例后，可以使用以下命令删除资源组、Web 应用以及所有相关资源。

```powershell
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzAppServicePlan](https://docs.microsoft.com/powershell/module/az.websites/new-azappserviceplan) | 创建应用服务计划。 |
| [New-AzWebApp](https://docs.microsoft.com/powershell/module/az.websites/new-azwebapp) | 创建 Web 应用。 |
| [Set-AzWebApp](https://docs.microsoft.com/powershell/module/az.websites/set-azwebapp) | 修改 Web 应用的配置。 |
| [Get-AzWebAppMetric](https://docs.microsoft.com/powershell/module/az.websites/get-azwebappmetric) | 获取 Web 应用的指标。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure PowerShell 示例](../samples-powershell.md)中找到 Azure 应用服务 Web 应用的其他 Azure Powershell 示例。
