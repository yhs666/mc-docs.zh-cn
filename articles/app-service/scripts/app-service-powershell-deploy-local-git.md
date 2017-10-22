---
title: "Azure PowerShell 脚本示例 - 从本地 Git 存储库创建 Web 应用并部署代码 | Azure"
description: "Azure PowerShell 脚本示例 - 从本地 Git 存储库创建 Web 应用并部署代码"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 5a927f23-8e70-45fd-9aae-980d4e7a007d
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: article
origin.date: 03/20/2017
ms.date: 10/30/2017
ms.author: v-yiso
ms.custom: mvc
ms.openlocfilehash: efdc9d1435dc243998d652e7e2192493a582108a
ms.sourcegitcommit: 6ef36b2aa8da8a7f249b31fb15a0fb4cc49b2a1b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2017
---
# <a name="create-a-web-app-and-deploy-code-from-a-local-git-repository"></a>从本地 Git 存储库创建 Web 应用并部署代码

此示例脚本使用其相关资源，在应用服务中创建 Web 应用，并在本地 Git 存储库中部署 Web 应用代码。

必要时，请使用 [Azure PowerShell 指南](https://docs.microsoft.com/powershell/azure/overview)中的说明安装 Azure PowerShell，并运行 `Login-AzureRmAccount -EnvironmentName AzureChinaCloud` 创建与 Azure 的连接。 此外，需将应用程序代码提交到本地 Git 存储库。

## <a name="sample-script"></a>示例脚本

```powershell
$gitdirectory="<Replace with path to local Git repo>"
$webappname="mywebapp$(Get-Random)"
$location="China North"

# Create a resource group.
New-AzureRmResourceGroup -Name myResourceGroup -Location $location

# Create an App Service plan in `Free` tier.
New-AzureRmAppServicePlan -Name $webappname -Location $location `
-ResourceGroupName myResourceGroup -Tier Free

# Create a web app.
New-AzureRmWebApp -Name $webappname -Location $location -AppServicePlan $webappname `
-ResourceGroupName myResourceGroup

# Configure GitHub deployment from your GitHub repo and deploy once.
$PropertiesObject = @{
    scmType = "LocalGit";
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName myResourceGroup `
-ResourceType Microsoft.Web/sites/config -ResourceName $webappname/web `
-ApiVersion 2015-08-01 -Force

# Get app-level deployment credentials
$xml = [xml](Get-AzureRmWebAppPublishingProfile -Name $webappname -ResourceGroupName myResourceGroup `
-OutputFile null)
$username = $xml.SelectNodes("//publishProfile[@publishMethod=`"MSDeploy`"]/@userName").value
$password = $xml.SelectNodes("//publishProfile[@publishMethod=`"MSDeploy`"]/@userPWD").value

# Add the Azure remote to your local Git respository and push your code
#### This method saves your password in the git remote. You can use a Git credential manager to secure your password instead.
git remote add azure "https://${username}:$password@$webappname.scm.chinacloudsites.cn"
git push azure master

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
| [Get-AzureRmWebAppPublishingProfile](https://docs.microsoft.com/powershell/module/azurerm.websites/get-azurermwebapppublishingprofile) | 获取 Web 应用的发布配置文件。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure PowerShell 示例](../app-service-powershell-samples.md)中找到 Azure 应用服务 Web 应用的其他 Azure Powershell 示例。
