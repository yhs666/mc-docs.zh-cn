---
title: "Azure PowerShell 脚本示例 - 从 GitHub 创建 Web 应用并部署代码 | Azure"
description: "Azure PowerShell 脚本示例 - 从 GitHub 创建 Web 应用并部署代码"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 0f9c8bc5-3789-4eb3-8deb-ae6e2200795a
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: article
origin.date: 03/20/2017
ms.date: 04/24/2017
ms.author: v-dazen
ms.translationtype: Human Translation
ms.sourcegitcommit: 4a18b6116e37e365e2d4c4e2d144d7588310292e
ms.openlocfilehash: 4b13619bee2cf9b4fd0352fa1dff013ad221981d
ms.contentlocale: zh-cn
ms.lasthandoff: 05/19/2017

---

# <a name="create-a-web-app-and-deploy-code-from-github"></a>从 GitHub 创建 Web 应用并部署代码

此示例脚本使用其相关资源，在应用服务中创建 Web 应用，然后从公共 GitHub 存储库部署 Web 应用代码（不进行连续部署）。 有关不进行连续部署的 GitHub 部署，请参阅[从 GitHub 使用连续部署创建 Web 应用](../app-service-continuous-deployment.md)。

必要时，请使用 [Azure PowerShell 指南](https://docs.microsoft.com/powershell/azure/overview)中的说明安装 Azure PowerShell。 此外，还需要指向含 web 应用代码的 GitHub 存储库的链接。

## <a name="sample-script"></a>示例脚本

```powershell
$gitrepo="<Replace with your GitHub repo URL>"
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

# Configure GitHub deployment from your GitHub repo and deploy once.
$PropertiesObject = @{
    repoUrl = "$gitrepo";
    branch = "master";
    isManualIntegration = "true";
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName $webappname `
-ResourceType Microsoft.Web/sites/sourcecontrols -ResourceName $webappname/web `
-ApiVersion 2015-08-01 -Force

```

## <a name="clean-up-deployment"></a>清理部署 

运行脚本示例后，可以使用以下命令删除资源组、Web 应用以及所有相关资源。

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzureRmAppServicePlan](https://docs.microsoft.com/powershell/module/azurerm.websites/new-azurermappserviceplan) | 创建应用服务计划。 |
| [New-AzureRmWebApp](https://docs.microsoft.com/powershell/module/azurerm.websites/new-azurermwebapp) | 创建 Web 应用。 |
| [Set-AzureRmResource](https://docs.microsoft.com/powershell/module/azurerm.resources/set-azurermresource) | 修改资源组中的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure PowerShell 示例](../app-service-powershell-samples.md)中找到 Azure 应用服务 Web 应用的其他 Azure Powershell 示例。

