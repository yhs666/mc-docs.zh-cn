---
title: "备份 Azure Stack | Microsoft Docs"
description: "在 Azure Stack 上使用备份就地执行按需备份。"
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 9565DDFB-2CDB-40CD-8964-697DA2FFF70A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 12/15/2017
ms.date: 03/01/2018
ms.author: v-junlch
ms.openlocfilehash: 414b8911cc0418a24dd9a1a531b93f5df2ed255d
ms.sourcegitcommit: 34925f252c9d395020dc3697a205af52ac8188ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="back-up-azure-stack"></a>备份 Azure Stack

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

在 Azure Stack 上使用备份就地执行按需备份。 如果需要启用基础结构备份服务，请参阅[从管理门户为 Azure Stack 启用备份](azure-stack-backup-enable-backup-console.md)。

> [!Note]  
>  Azure Stack 工具包含 **Start-AzSBackup** cmdlet。 有关安装工具的说明，请参阅[在 Azure Stack 中启动并运行 PowerShell](/azure-stack/azure-stack-powershell-configure-quickstart)。

## <a name="start-azure-stack-backup"></a>启动 Azure Stack 备份

在操作员管理环境中使用权限提升的提示符打开 Windows PowerShell，并运行以下命令：

```powershell
    cd C:\tools\AzureStack-Tools-master\Connect
    Import-Module .\AzureStack.Connect.psm1

    cd C:\tools\AzureStack-Tools-master\Infrastructure
    Import-Module .\AzureStack.Infra.psm1 
    
    Start-AzSBackup -Location $location.Name
```

## <a name="confirm-backup-completed-in-the-administration-portal"></a>在管理门户中确认已完成的备份

1. 打开 Azure Stack 管理门户（网址为 [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external)）。
2. 选择“更多服务” > “基础结构备份”。 在“基础结构备份”边栏选项卡中选择“配置”。
3. 在“可用备份”列表中查找备份的**名称**和**完成日期**。
4. 验证**状态**是否为“成功”。

还可以从管理门户确认已完成的备份。 导航到 `\MASBackup\<datetime>\<backupid>\BackupInfo.xml`

## <a name="next-steps"></a>后续步骤

- 详细了解从数据丢失事件恢复的工作流程。 请参阅[从灾难性数据丢失中恢复](azure-stack-backup-recover-data.md)。

