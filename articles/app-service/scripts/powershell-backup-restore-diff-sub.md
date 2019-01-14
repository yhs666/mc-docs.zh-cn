---
title: Azure PowerShell 脚本示例 - 将应用备份还原到其他订阅
description: Azure PowerShell 脚本示例 - 从另一订阅的备份中还原 Web 应用
services: app-service\web
documentationcenter: ''
author: msangapu
manager: jpconnoc
editor: ''
tags: azure-service-management
ms.assetid: a2a27d94-d378-4c17-a6a9-ae1e69dc4a72
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
origin.date: 11/21/2018
ms.date: 01/21/2019
ms.author: v-biyu
ms.custom: seodec18
ms.openlocfilehash: 81e0f42f98d8e8708f719063202fa5bfc1d4aa25
ms.sourcegitcommit: 90d5f59427ffa599e8ec005ef06e634e5e843d1e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2019
ms.locfileid: "54083905"
---
# <a name="restore-a-web-app-from-a-backup-in-another-subscription-using-powershell"></a>使用 PowerShell 从另一订阅中的备份还原 Web 应用

此示例脚本从现有的 Web 应用中检索以前已完成的备份，然后将其还原到另一订阅中的 Web 应用。 

必要时，请使用 [Azure PowerShell 指南](https://docs.microsoft.com/en-us/powershell/azure/overview)中的说明安装 Azure PowerShell，并运行 `Connect-AzureRmAccount` 创建与 Azure 的连接。 

## <a name="sample-script"></a>示例脚本

$resourceGroupNameSub1 = "<替换为你的组名称>" $resourceGroupNameSub2 = "<替换为所需的新组名称>" $webAppNameSub1 = "<替换为你的应用名称>" $webAppNameSub2 = "<替换为所需的新应用名称>" $appServicePlanSub2 = "<替换为所需的新计划名称>" $locationSub2 = "China East"


# <a name="log-into-the-subscription-with-the-backup"></a>登录到包含备份的订阅
Add-AzureRmAccount

# <a name="list-statuses-of-all-backups-that-are-complete-or-currently-executing"></a>列出已完成或当前正在执行的所有备份的状态。
Get-AzureRmWebAppBackupList -ResourceGroupName $resourceGroupNameSub1 -Name $webAppNameSub1

# <a name="note-the-backupid-property-of-the-backup-you-want-to-restore"></a>请注意要还原的备份的 BackupID 属性

# <a name="get-the-backup-object-that-you-want-to-restore-by-specifying-the-backupid"></a>通过指定 BackupID 获取要还原的备份对象
$backup = (Get-AzureRmWebAppBackupList -ResourceGroupName $resourceGroupNameSub1 -Name $webAppNameSub1 | where {$_.BackupId -eq <replace-with-BackupID>}) 

# <a name="log-into-the-subscription-that-you-want-to-restore-the-app-to"></a>登录到要将应用还原到的订阅
Add-AzureRmAccount

# <a name="create-a-new-web-app"></a>创建新的 Web 应用
New-AzureRmWebApp -ResourceGroupName $resourceGroupNameSub2 -AppServicePlan $appServicePlanSub2 -Name $webAppNameSub2 -Location $locationSub2

# <a name="restore-the-app-by-overwriting-it-with-the-backup-data"></a>通过使用备份数据重写应用来还原应用
Restore-AzureRmWebAppBackup -ResourceGroupName $resourceGroupNameSub2 -Name $webAppNameSub2 -StorageAccountUrl $backup.StorageAccountUrl -BlobName $backup.BlobName -Overwrite

## <a name="clean-up-deployment"></a>清理部署 

如果不再需要 Web 应用，请使用以下命令删除资源组、Web 应用和所有相关的资源。

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroupName -Force
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [Add-AzureRmAccount](https://docs.microsoft.com/en-us/powershell/module/azurerm.profile/add-azurermaccount) | 添加一个经身份验证的帐户，用于 Azure 资源管理器 cmdlet 请求。  |
| [Get-AzureRmWebAppBackupList](https://docs.microsoft.com/en-us/powershell/module/azurerm.websites/get-azurermwebappbackuplist) | 获取 Web 应用的备份列表。 |
| [New-AzureRmWebApp](https://docs.microsoft.com/en-us/powershell/module/azurerm.websites/new-azurermwebapp) | 创建 Web 应用 |
| [Restore-AzureRmWebAppBackup](https://docs.microsoft.com/en-us/powershell/module/azurerm.websites/restore-azurermwebappbackup) | 从以前完成的备份中还原 Web 应用。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/en-us/powershell/azure/overview)。

可以在 [Azure PowerShell 示例](../samples-powershell.md)中找到 Azure 应用服务 Web 应用的其他 Azure Powershell 示例。
