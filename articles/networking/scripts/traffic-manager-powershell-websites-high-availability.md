---
title: Azure PowerShell 脚本示例 - 为实现应用程序的高可用性路由流量
description: Azure PowerShell 脚本示例 - 为实现应用程序的高可用性路由流量
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: timlt
editor: georgewallace
tags: azure-infrastructure
ms.assetid: ''
ms.service: traffic-manager
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: traffic-manager
origin.date: 05/16/2017
ms.date: 03/11/2019
ms.author: v-biyu
ms.openlocfilehash: 941dfe06e6432ff311109abff98569e6679e2e19
ms.sourcegitcommit: b8fb6890caed87831b28c82738d6cecfe50674fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/29/2019
ms.locfileid: "58626035"
---
# <a name="route-traffic-for-high-availability-of-applications"></a>为实现应用程序的高可用性路由流量

此脚本将创建一个资源组、两个应用服务计划、两个 Web 应用、一个流量管理器配置文件和两个流量管理器终结点。 流量管理器将流量引导到一个区域（作为主要区域）中的应用程序；主要区域中的应用程序不可用时，引导到次要区域。 执行脚本前，必须将 MyWebApp、MyWebAppL1 和 MyWebAppL2 值更改为 Azure 内的唯一值。 运行脚本后，可以使用 URL mywebapp.trafficmanager.net 访问主要区域中的应用。

必要时，请使用 [Azure PowerShell 指南](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)中的说明安装 Azure PowerShell，并运行 `Connect-AzAccount` 创建与 Azure 的连接。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

# <a name="variables-for-common-values"></a>常见值的变量
$rgName1="MyResourceGroup1" $rgName2="MyResourceGroup2" $location1="chinaeast" $location2="chinaeast2"

# <a name="the-values-of-the-variables-below-must-be-unique-replace-with-your-own-names"></a>以下变量的值必须唯一（请替换为自己的名称）。
$webApp1="mywebapp$(Get-Random)" $webApp2="mywebapp$(Get-Random)" $webAppL1="MyWebAppL1" $webAppL2="MyWebAppL2"

# <a name="create-a-resource-group-in-location-one"></a>在位置 1 中创建资源组。
New-AzResourceGroup -Name $rgName1 -Location $location1

# <a name="create-a-resource-group-in-location-two"></a>在位置 2 中创建资源组。
New-AzResourceGroup -Name $rgName2 -Location $location2

# <a name="create-a-website-deployed-from-github-in-both-regions-replace-with-your-own-github-url"></a>在两个区域中创建从 GitHub 部署的网站（请将其替换为你自己的 GitHub URL）。
$gitrepo="<https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git>"

# <a name="create-a-hosting-plan-and-website-and-deploy-it-in-location-one-requires-standard-1-minimum-sku"></a>创建托管计划和网站，并将其部署到位置 1（需要标准 1 最小 SKU）。

$appServicePlan = New-AzAppServicePlan -Name $webappl1 -ResourceGroupName $rgName1 ` -Location $location1 -Tier Standard 

$web1 = New-AzWebApp -ResourceGroupName $rgname1 -Name $webApp1 -Location $location1 ` -AppServicePlan $webappl1

# <a name="configure-github-deployment-from-your-github-repo-and-deploy-once"></a>从 GitHub 存储库配置 GitHub 部署并部署一次。
$PropertiesObject = @{ repoUrl = "$gitrepo"; branch = "master"; isManualIntegration = "true"; }

Set-AzResource -PropertyObject $PropertiesObject -ResourceGroupName $rgname1 `
-ResourceType Microsoft.Web/sites/sourcecontrols -ResourceName $webapp1/web ` -ApiVersion 2015-08-01 -Force

# <a name="create-a-hosting-plan-and-website-and-deploy-it-in-location-two-requires-standard-1-minimum-sku"></a>创建托管计划和网站，并将其部署到位置 2（需要标准 1 最小 SKU）。

$appServicePlan = New-AzAppServicePlan -Name $webappl2 -ResourceGroupName $rgName2 ` -Location $location2 -Tier Standard 

$web2 = New-AzWebApp -ResourceGroupName $rgname2 -Name $webApp2 ` -Location $location2 -AppServicePlan $webappl2

$PropertiesObject = @{ repoUrl = "$gitrepo"; branch = "master"; isManualIntegration = "true"; }

Set-AzResource -PropertyObject $PropertiesObject -ResourceGroupName $rgname2 `
  -ResourceType Microsoft.Web/sites/sourcecontrols -ResourceName $webapp2/web ` -ApiVersion 2015-08-01 -Force

# <a name="create-a-traffic-manager-profile"></a>创建流量管理器配置文件。
$tm = New-AzTrafficManagerProfile -Name 'MyTrafficManagerProfile' -ResourceGroupName $rgname1 `
  -TrafficRoutingMethod Priority -RelativeDnsName $web1.SiteName -Ttl 60 ` -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath /


# <a name="create-an-endpoint-for-the-location-one-website-deployment-and-set-it-as-the-priority-target"></a>为位置 1 网站部署创建终结点，并将其设置为优先目标。
$endpoint = New-AzTrafficManagerEndpoint -Name 'MyEndPoint1' -ProfileName $tm.Name `
  -ResourceGroupName $rgname1 -Type AzureEndpoints -Priority 1 ` -TargetResourceId $web1.Id -EndpointStatus Enabled

# <a name="create-an-endpoint-for-the-location-two-website-deployment-and-set-it-as-the-secondary-target"></a>为位置 2 网站部署创建终结点，并将其设置为辅助目标。
$endpoint2 = New-AzTrafficManagerEndpoint -Name 'MyEndPoint2' -ProfileName $tm.Name `
  -ResourceGroupName $rgname1 -Type AzureEndpoints -Priority 2 ` -TargetResourceId $web2.Id -EndpointStatus Enabled


运行以下命令来删除资源组、VM 和所有相关资源。

```powershell
Remove-AzResourceGroup -Name myResourceGroup1
Remove-AzResourceGroup -Name myResourceGroup2
```


## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、Web 应用、流量管理器配置文件和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/zh-cn/powershell/module/az.resources/new-azresourcegroup)  | 创建用于存储所有资源的资源组。 |
| [New-AzAppServicePlan](https://docs.microsoft.com/zh-cn/powershell/module/az.websites/new-azappserviceplan) | 创建应用服务计划。 这与 Azure Web 应用的服务器场类似。 |
| [New-AzWebApp](https://docs.microsoft.com/zh-cn/powershell/module/az.websites/new-azwebapp) | 创建应用服务计划中的 Azure Web 应用。 |
| [Set-AzResource](https://docs.microsoft.com/zh-cn/powershell/module/az.resources/new-azresource) | 创建应用服务计划中的 Azure Web 应用。 |
| [New-AzTrafficManagerProfile](https://docs.microsoft.com/zh-cn/powershell/module/az.trafficmanager/new-aztrafficmanagerprofile) | 创建 Azure 流量管理器配置文件。 |
| [New-AzTrafficManagerEndpoint](https://docs.microsoft.com/zh-cn/powershell/module/az.trafficmanager/new-aztrafficmanagerendpoint) | 将终结点添加到 Azure 流量管理器配置文件。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可在 [Azure 网络概述文档](../powershell-samples.md?toc=%2fnetworking%2ftoc.json)中找到其他网络 PowerShell 脚本示例。
