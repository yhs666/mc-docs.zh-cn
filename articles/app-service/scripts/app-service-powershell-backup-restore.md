---
title: Azure PowerShell 脚本示例 - 从备份恢复 Web 应用
description: Azure PowerShell 脚本示例 - 从备份恢复 Web 应用
services: app-service\web
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: a2a27d94-d378-4c17-a6a9-ae1e69dc4a72
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
origin.date: 11/21/2018
ms.date: 12/31/2018
ms.author: v-biyu
ms.custom: seodec18
ms.openlocfilehash: 2034a90e2b47ed2bc6f2badd1d26de555bd67252
ms.sourcegitcommit: 80c59ae1174d71509b4aa64a28a98670307a5b38
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/21/2018
ms.locfileid: "53735208"
---
# <a name="restore-a-web-app-from-a-backup-using-azure-powershell"></a>使用 PowerShell 从备份中还原 Web 应用

此示例脚本从现有的 Web 应用中检索以前已完成的备份，然后通过重写其内容将其还原。 

必要时，请使用 [Azure PowerShell 指南](https://docs.microsoft.com/en-us/powershell/azure/overview)中的说明安装 Azure PowerShell，并运行 `Connect-AzureRmAccount` 创建与 Azure 的连接。 

## <a name="sample-script"></a>示例脚本

$resourceGroupName = "myResourceGroup" $webappname = "<replace-with-your-app-name>"


# <a name="list-statuses-of-all-backups-that-are-complete-or-currently-executing"></a>列出已完成或当前正在执行的所有备份的状态。
Get-AzureRmWebAppBackupList -ResourceGroupName $resourceGroupName -Name $webappname

# <a name="note-the-backupid-property-of-the-backup-you-want-to-restore"></a>请注意要还原的备份的 BackupID 属性

# <a name="get-the-backup-object-that-you-want-to-restore-by-specifying-the-backupid"></a>通过指定 BackupID 获取要还原的备份对象
$backup = (Get-AzureRmWebAppBackupList -ResourceGroupName $resourceGroupName -Name $webappname | where {$_.BackupId -eq <replace-with-BackupID>}) 

# <a name="restore-the-app-by-overwriting-it-with-the-backup-data"></a>通过使用备份数据重写应用来还原应用
$backup | Restore-AzureRmWebAppBackup -Overwrite

## <a name="clean-up-deployment"></a>清理部署 

如果不再需要 Web 应用，请使用以下命令删除资源组、Web 应用和所有相关的资源。

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroupName -Force
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [Get-AzureRmWebAppBackupList](https://docs.microsoft.com/en-us/powershell/module/azurerm.websites/get-azurermwebappbackuplist) | 获取 Web 应用的备份列表。 |
| [Restore-AzureRmWebAppBackup](https://docs.microsoft.com/en-us/powershell/module/azurerm.websites/restore-azurermwebappbackup) | 从以前完成的备份中还原 Web 应用。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/en-us/powershell/azure/overview)。

可以在 [Azure PowerShell 示例](../app-service-powershell-samples.md)中找到 Azure 应用服务 Web 应用的其他 Azure Powershell 示例。
