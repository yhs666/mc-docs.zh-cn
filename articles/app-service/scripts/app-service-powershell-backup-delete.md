---
title: Azure PowerShell 脚本示例 - 删除 Web 应用的备份 | Microsoft Docs
description: Azure PowerShell 脚本示例 - 删除 Web 应用的备份
services: app-service\web
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
tags: azure-service-management
ms.assetid: ebcadb49-755d-4202-a5eb-f211827a9168
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
origin.date: 10/30/2017
ms.author: v-yiso
ms.date: 12/04/2017
ms.custom: mvc
ms.openlocfilehash: da1a4912ef671fc908182ba97e9bc4edda0d724c
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52651586"
---
# <a name="delete-a-backup-for-a-web-app"></a>删除 Web 应用的备份

此示例脚本使用其相关资源，在应用服务中创建 Web 应用，然后为其创建一次性备份。 

若要运行此脚本，需要 Web 应用的现有备份。 若要创建备份，请参阅[备份 Web 应用](app-service-powershell-backup-onetime.md)或[创建 Web 应用的计划备份](app-service-powershell-backup-scheduled.md)。

## <a name="sample-script"></a>示例脚本

```powershell
$resourceGroupName = "myResourceGroup"
$webappname = "<replace-with-your-app-name>"

# List statuses of all backups that are complete or currently executing.
Get-AzureRmWebAppBackupList -ResourceGroupName $resourceGroupName -Name $webappname

# Note the BackupID property of the backup you want to delete

# Delete the backup by specifying the BackupID
Remove-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $webappname `
-BackupId <replace-with-BackupID>
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
| [Get-AzureRmWebAppBackupList](https://docs.microsoft.com/en-us/powershell/module/azurerm.websites/get-azurermwebappbackuplist) | 获取 Web 应用的备份列表。 |
| [Remove-AzureRmWebAppBackup](https://docs.microsoft.com/en-us/powershell/module/azurerm.websites/remove-azurermwebappbackup) | 删除 Web 应用的指定备份。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/en-us/powershell/azure/overview)。

可以在 [Azure PowerShell 示例](../app-service-powershell-samples.md)中找到 Azure 应用服务 Web 应用的其他 Azure Powershell 示例。
