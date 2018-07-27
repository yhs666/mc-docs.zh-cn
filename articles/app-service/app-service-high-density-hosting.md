---
title: 在 Azure 应用服务上使用按应用缩放进行高密度托管 | Microsoft Docs
description: 在 Azure 应用服务上进行高密度托管
author: btardif
manager: erikre
editor: ''
services: app-service\web
documentationcenter: ''
ms.assetid: a903cb78-4927-47b0-8427-56412c4e3e64
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
origin.date: 01/22/2018
ms.date: 07/30/2018
ms.author: v-yiso
ms.openlocfilehash: bedee80292c4123496866fed04103bfda13c6ac6
ms.sourcegitcommit: 6d4ae5e324dbad3cec8f580276f49da4429ba1a7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2018
ms.locfileid: "39167738"
---
# <a name="high-density-hosting-on-azure-app-service-using-per-app-scaling"></a>在 Azure 应用服务上使用按应用缩放进行高密度托管
默认情况下，通过缩放应用服务应用在其中运行的[应用服务计划](azure-web-sites-web-hosting-plans-in-depth-overview.md)来缩放这些应用。 当多个应用在同一个应用服务计划中运行时，每个横向扩展实例会在计划中运行所有应用。

可以在应用服务计划级别启用按应用缩放。 按应用缩放在缩放应用时独立于所属的应用服务计划。 这样，可以将一个应用服务计划扩展到 10 个实例，而将一个应用设置为仅使用 5 个实例。

> [!NOTE]
> 按应用缩放仅适用于标准、高级和独立定价层。
>

## <a name="per-app-scaling-using-powershell"></a>使用 PowerShell 进行按应用缩放

通过将 ```-perSiteScaling $true``` 属性传入 ```New-AzureRmAppServicePlan``` commandlet，创建按应用缩放的计划

```powershell
New-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan `
                            -Location $Location `
                            -Tier Premium -WorkerSize Small `
                            -NumberofWorkers 5 -PerSiteScaling $true
```

若要通过按应用缩放更新现有的应用服务计划，请执行以下操作： 

- 获取目标计划 ```Get-AzureRmAppServicePlan```
- 在本地修改属性 ```$newASP.PerSiteScaling = $true```
- 将更改发布回 Azure ```Set-AzureRmAppServicePlan``` 

```powershell
# Get the new App Service Plan and modify the "PerSiteScaling" property.
$newASP = Get-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan
$newASP

#Modify the local copy to use "PerSiteScaling" property.
$newASP.PerSiteScaling = $true
$newASP
    
#Post updated app service plan back to azure
Set-AzureRmAppServicePlan $newASP
```

在应用级别，配置应用可以在应用服务计划中使用的实例数。

在以下示例中，无论基础应用服务计划横向扩展到多少个实例，应用都被限制为两个实例。

```powershell
# Get the app we want to configure to use "PerSiteScaling"
$newapp = Get-AzureRmWebApp -ResourceGroupName $ResourceGroup -Name $webapp
    
# Modify the NumberOfWorkers setting to the desired value.
$newapp.SiteConfig.NumberOfWorkers = 2
    
# Post updated app back to azure
Set-AzureRmWebApp $newapp
```

> [!IMPORTANT]
> `$newapp.SiteConfig.NumberOfWorkers` 不同于 `$newapp.MaxNumberOfWorkers`。 按应用缩放使用 `$newapp.SiteConfig.NumberOfWorkers` 来确定应用的缩放特征。

## <a name="per-app-scaling-using-azure-resource-manager"></a>使用 Azure 资源管理器的按应用缩放

以下 Azure 资源管理器模板创建：

- 扩展到 10 个实例的应用服务计划
- 已配置为最多可扩展到 5 个实例的应用。

应用服务计划将 **PerSiteScaling** 属性设置为 true `"perSiteScaling": true`。 应用将要使用的**辅助角色数量**设置为 5 `"properties": { "numberOfWorkers": "5" }`。

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters":{
        "appServicePlanName": { "type": "string" },
        "appName": { "type": "string" }
        },
    "resources": [
    {
        "comments": "App Service Plan with per site perSiteScaling = true",
        "type": "Microsoft.Web/serverFarms",
        "sku": {
            "name": "P1",
            "tier": "Premium",
            "size": "P1",
            "family": "P",
            "capacity": 10
            },
        "name": "[parameters('appServicePlanName')]",
        "apiVersion": "2015-08-01",
        "location": "China East",
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "perSiteScaling": true
        }
    },
    {
        "type": "Microsoft.Web/sites",
        "name": "[parameters('appName')]",
        "apiVersion": "2015-08-01-preview",
        "location": "China East",
        "dependsOn": [ "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" ],
        "properties": { "serverFarmId": "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" },
        "resources": [ {
                "comments": "",
                "type": "config",
                "name": "web",
                "apiVersion": "2015-08-01",
                "location": "China East",
                "dependsOn": [ "[resourceId('Microsoft.Web/Sites', parameters('appName'))]" ],
                "properties": { "numberOfWorkers": "5" }
            } ]
        }]
}
```


## <a name="next-steps"></a>后续步骤

- [Azure 应用服务计划深入概述](azure-web-sites-web-hosting-plans-in-depth-overview.md)
