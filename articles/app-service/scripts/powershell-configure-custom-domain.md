---
title: Azure PowerShell 脚本示例 - 将自定义域分配到 Web 应用 | Azure
description: Azure PowerShell 脚本示例 - 将自定义域分配到 Web 应用
services: app-service\web
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: 356f5af9-f62e-411c-8b24-deba05214103
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
origin.date: 03/20/2017
ms.date: 01/21/2019
ms.author: v-biyu
ms.custom: seodec18
ms.openlocfilehash: 360986d805584112c2f1195cb6872ebde0746a69
ms.sourcegitcommit: 90d5f59427ffa599e8ec005ef06e634e5e843d1e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2019
ms.locfileid: "54083909"
---
# <a name="assign-a-custom-domain-to-a-web-app-using-powershell"></a>使用 PowerShell 将自定义域分配到 Web 应用

此示例脚本使用其相关资源，在应用服务中创建 Web 应用，并将 `www.<yourdomain>` 映射到它。 

必要时，请使用 [Azure PowerShell 指南](https://docs.microsoft.com/powershell/azure/overview)中的说明安装 Azure PowerShell，并运行 `Login-AzureRmAccount -EnvironmentName AzureChinaCloud` 创建与 Azure 的连接。 此外，还需有权访问域注册机构的 DNS 配置页。

## <a name="sample-script"></a>示例脚本

```powershell
$fqdn="<Replace with your custom domain name>"
$webappname="mywebapp$(Get-Random)"
$location="China North"

# Create a resource group.
New-AzureRmResourceGroup -Name $webappname -Location $location

# Create an App Service plan in Free tier.
New-AzureRmAppServicePlan -Name $webappname -Location $location `
-ResourceGroupName $webappname -Tier Free

# Create a web app.
New-AzureRmWebApp -Name $webappname -Location $location -AppServicePlan $webappname `
-ResourceGroupName $webappname

Write-Host "Configure a CNAME record that maps $fqdn to $webappname.chinacloudsites.cn"
Read-Host "Press [Enter] key when ready ..."

# Before continuing, go to your DNS configuration UI for your custom domain and follow the 
# instructions at https://docs.azure.cn/app-service-web/web-sites-custom-domain-name#step-2-create-the-dns-records to configure a CNAME record for the 
# hostname "www" and point it your web app's default domain name.

# Upgrade App Service plan to Shared tier (minimum required by custom domains)
Set-AzureRmAppServicePlan -Name $webappname -ResourceGroupName $webappname `
-Tier Shared

# Add a custom domain name to the web app. 
Set-AzureRmWebApp -Name $webappname -ResourceGroupName $webappname `
-HostNames @($fqdn,"$webappname.chinacloudsites.cn")

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
| [Set-AzureRmAppServicePlan](https://docs.microsoft.com/powershell/module/azurerm.websites/set-azurermappserviceplan) | 修改应用服务计划以更改其定价层。 |
| [Set-AzureRmWebApp](https://docs.microsoft.com/powershell/module/azurerm.websites/set-azurermwebapp) | 修改 Web 应用的配置。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure PowerShell 示例](../samples-powershell.md)中找到 Azure 应用服务 Web 应用的其他 Azure Powershell 示例。
