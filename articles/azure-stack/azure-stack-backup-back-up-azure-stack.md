---
title: 备份 Azure Stack | Microsoft Docs
description: 在 Azure Stack 上使用备份就地执行按需备份。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 9565DDFB-2CDB-40CD-8964-697DA2FFF70A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 05/08/2017
ms.date: 06/26/2018
ms.author: v-junlch
ms.reviewer: hectorl
ms.openlocfilehash: 35417a35f613dfce858582d6ec7e78628138b68c
ms.sourcegitcommit: 8a17603589d38b4ae6254bb9fc125d668442ea1b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2018
ms.locfileid: "37027063"
---
# <a name="back-up-azure-stack"></a>备份 Azure Stack

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

在 Azure Stack 上使用备份就地执行按需备份。 如果需要启用基础结构备份服务，请参阅[从管理门户为 Azure Stack 启用备份](azure-stack-backup-enable-backup-console.md)。

## <a name="setup-rm-environment-and-log-into-the-operator-management-endpoint"></a>设置 Rm 环境并登录到操作员管理终结点

> [!Note]  
>  有关配置 PowerShell 环境的说明，请参阅[安装适用于 Azure Stack 的 PowerShell](azure-stack-powershell-install.md)。

通过添加环境变量编辑以下 PowerShell 脚本。 运行更新的脚本以设置 RM 环境并登录到操作员管理终结点。

| 变量    | 说明 |
|---          |---          |
| $TenantName | Azure Active Directory 租户名称。 |
| 操作员帐户名称        | Azure Stack 操作员帐户名称。 |
| Azure 资源管理器终结点 | Azure 资源管理器的 URL。 |

   ```powershell
   # Specify Azure Active Directory tenant name
    $TenantName = "contoso.partner.onmschina.cn"
    
    # Set the module repository and the execution policy
    Set-PSRepository `
      -Name "PSGallery" `
      -InstallationPolicy Trusted
    
    Set-ExecutionPolicy RemoteSigned `
      -force
    
    # Configure the Azure Stack operator’s PowerShell environment.
    Add-AzureRMEnvironment `
      -Name "AzureStackAdmin" `
      -ArmEndpoint "https://adminmanagement.seattle.contoso.com"
    
    Set-AzureRmEnvironment `
      -Name "AzureStackAdmin" `
      -GraphAudience "https://graph.chinacloudapi.cn/"
    
    $TenantID = Get-AzsDirectoryTenantId `
      -AADTenantName $TenantName `
      -EnvironmentName AzureStackAdmin
    
    # Sign-in to the operator's console.
    Add-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $TenantID
    
   ```

## <a name="start-azure-stack-backup"></a>启动 Azure Stack 备份

```powershell
$location = Get-AzsLocation
Start-AzSBackup -Location $location.Name
```

## <a name="confirm-backup-completed-via-powershell"></a>确认已通过 PowerShell 完成备份

```powershell
Get-AzsBackup -Location $location.Name | Select-Object -ExpandProperty BackupInfo
```

- 结果应类似于以下输出：

  ```powershell
      backupDataVersion :
      backupId          : xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx
      roleStatus        : {@{roleName=NRP; status=Succeeded}, @{roleName=SRP; status=Succeeded}, @{roleName=CRP; status=Succeeded}, @{roleName=KeyVaultInternalControlPlane; status=Succeeded}...}
      status            : Succeeded
      createdDateTime   : 2018-05-03T12:16:50.3876124Z
      timeTakenToCreate : PT22M54.1714666S
      stampVersion      :
      oemVersion        :
      deploymentID      :
  ```

## <a name="confirm-backup-completed-in-the-administration-portal"></a>在管理门户中确认已完成的备份

1. 打开 Azure Stack 管理门户 ([https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external))。
2. 选择“更多服务” > “基础结构备份”。 在“基础结构备份”边栏选项卡中选择“配置”。
3. 在“可用备份”列表中查找备份的**名称**和**完成日期**。
4. 验证**状态**是否为“成功”。

<!-- You can also confirm the backup completed from the administration portal. Navigate to `\MASBackup\<datetime>\<backupid>\BackupInfo.xml`

In ‘Confirm backup completed’ section, the path at the end doesn’t make sense (ie relative to what, datetime format, etc?)
\MASBackup\<datetime>\<backupid>\BackupInfo.xml -->


## <a name="next-steps"></a>后续步骤

- 详细了解从数据丢失事件恢复的工作流程。 请参阅[从灾难性数据丢失中恢复](azure-stack-backup-recover-data.md)。

<!-- Update_Description: wording update -->