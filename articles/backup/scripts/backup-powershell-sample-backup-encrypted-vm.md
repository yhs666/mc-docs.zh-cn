---
title: "Azure PowerShell 脚本示例 - 备份 Azure 虚拟机 | Microsoft Docs"
description: "Azure PowerShell 脚本示例 - 备份 Azure 虚拟机"
services: backup
documentationcenter: 
author: alexchen2016
manager: digimobile
editor: 
tags: 
ms.assetid: 
ms.service: backup
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
origin.date: 09/07/2017
ms.date: 11/02/2017
ms.author: v-junlch
ms.custom: mvc
ms.openlocfilehash: 65fd458d80bf93177ffd668ca55075bb2e8a8ccc
ms.sourcegitcommit: c2be8d831d87f6a4d28c5950bebb2c7b8b6760bf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2017
---
# <a name="back-up-an-encrypted-azure-virtual-machine-with-powershell"></a>使用 PowerShell 备份已加密 Azure 虚拟机

此脚本为已加密 Azure 虚拟机创建包含异地冗余存储 (GRS) 的恢复服务保管库。 默认保护策略已应用到此保管库。 此策略为虚拟机生成每日备份，并将每个备份保留 30 天。 该脚本还将触发虚拟机的初始恢复点，并将该恢复点保留 365 天。 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

## <a name="sample-script"></a>示例脚本

```powershell
# Edit these global variables with your unique Recovery Services Vault name, resource group name and location
$rsVaultName = "myRsVault"
$rgName = "myResourceGroup"
$location = "chinanorth"

# Register the Recovery Services provider and create a resource group
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
New-AzureRmResourceGroup -Location $location -Name $rgName

# Create a Recovery Services Vault and set its storage redundancy type
New-AzureRmRecoveryServicesVault `
    -Name $rsVaultName `
    -ResourceGroupName $rgName `
    -Location $location 
$vault1 = Get-AzureRmRecoveryServicesVault –Name $rsVaultName
Set-AzureRmRecoveryServicesBackupProperties ` 
    -Vault $vault1 `
    -BackupStorageRedundancy GeoRedundant
    
# Set Recovery Services Vault context and create protection policy
Get-AzureRmRecoveryServicesVault -Name $rsVaultName | Set-AzureRmRecoveryServicesVaultContext 
$schPol = Get-AzureRmRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureVM"
$retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
New-AzureRmRecoveryServicesBackupProtectionPolicy `
    -Name "NewPolicy" `
    -WorkloadType "AzureVM" ` 
    -RetentionPolicy $retPol `
    -SchedulePolicy $schPol
    
# Provide permissions to Azure Backup to access key vault and enable backup on the VM
Set-AzureRmKeyVaultAccessPolicy `
    -VaultName "KeyVaultName" `
    -ResourceGroupName "KyeVault-RGName" ` 
    -PermissionsToKeys backup,get,list `
    -PermissionsToSecrets backup,get,list ` 
    -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
$pol = Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" `
Enable-AzureRmRecoveryServicesBackupProtection `
    -Policy $pol `
    -Name "myVM" `
    -ResourceGroupName "VM-RGName" 
    
# Modify protection policy
$retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
$retPol.DailySchedule.DurationCountInDays = 365
$pol = Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
Set-AzureRmRecoveryServicesBackupProtectionPolicy `
    -Policy $pol `
    -RetentionPolicy $RetPol
    
# Trigger a backup and monitor backup job
$namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -FriendlyName "myVM"
$item = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
$job = Backup-AzureRmRecoveryServicesBackupItem -Item $item
$joblist = Get-AzureRmRecoveryservicesBackupJob –Status "InProgress"
Wait-AzureRmRecoveryServicesBackupJob `
        -Job $joblist[0] `
        -Timeout 43200
```

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、VM 和所有相关资源。

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建部署。 表中的每一项均链接到特定于命令的文档。

| 命令 | 说明 | 
|---|---| 
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) | 创建用于存储所有资源的资源组。 | 
| [New-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/New-AzureRmRecoveryServicesVault) | 创建用于存储备份的恢复服务保管库。 | 
| [Set-AzureRmRecoveryServicesBackupProperties](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/Set-AzureRmRecoveryServicesBackupProperties) | 设置恢复服务保管库的备份存储属性。 | 
| [New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)| 在恢复服务保管库中使用计划策略和保留策略创建保护策略。 | 
| [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) | 设置对 Key Vault 的权限，授予服务主体访问加密密钥的权限。 | 
| [Enable-AzureRmRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection) | 使用指定的备份保护策略为某一项启用备份。 | 
| [Set-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy)| 修改现有备份保护策略。 | 
| [Backup-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem) | 为未绑定到备份计划的受保护 Azure 备份项启动备份。 |
| [Wait-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob) | 等待 Azure 备份作业完成。 | 
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) | 删除资源组及其中包含的所有资源。 | 

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。


