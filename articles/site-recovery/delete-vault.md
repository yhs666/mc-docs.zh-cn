---
title: "删除 Site Recovery 保管库"
description: "了解如何根据 Site Recovery 方案删除 Azure Site Recovery 保管库。"
service: site-recovery
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
origin.date: 07/04/2017
ms.date: 08/28/2017
ms.author: v-yeche
ms.openlocfilehash: 4d7f7dea4e9e1d16adbadf169b93352b3725d365
ms.sourcegitcommit: 1ca439ddc22cb4d67e900e3f1757471b3878ca43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2017
---
# <a name="delete-a-site-recovery-vault"></a>删除 Site Recovery 保管库
依赖项可能会阻止删除 Azure Site Recovery 保管库。 需要采取的措施因 Site Recovery 方案而异：VMware 到 Azure、Hyper-V（含和不含 System Center Virtual Machine Manager）到 Azure 和 Azure 备份。 若要删除用于 Azure 备份的保管库，请参阅[删除 Azure 备份保管库](../backup/backup-azure-delete-vault.md)。

>[!Important]
>若要测试产品，并对数据丢失并不在意，请使用强制删除方法，快速删除保管库及其所有依赖项。

> PowerShell 命令会删除保管库中的所有内容，并且无法撤消这一步。

## <a name="use-powershell-to-force-delete-the-vault"></a>使用 PowerShell 强制删除保管库 

若要删除 Site Recovery 保管库（即使有受保护的项，也不例外），请运行以下命令：

    Login-AzureRmAccount -EnvironmentName AzureChinaCloud

    Select-AzureRmSubscription -SubscriptionName "XXXXX"

    $vault = Get-AzureRmSiteRecoveryVault -Name "vaultname"

    Remove-AzureRmSiteRecoveryVault -Vault $vault

## <a name="delete-a-site-recovery-vault"></a>删除 Site Recovery 保管库 
若要删除保管库，请按照方案对应的推荐步骤操作。

### <a name="vmware-vms-to-azure"></a>VMware VM 到 Azure

1. 按照[禁用 VMware 保护](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server)中的步骤操作，删除所有受保护的 VM。

2. 按照[删除复制策略](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy)中的步骤操作，删除所有复制策略。

3. 按照[删除 vCenter](site-recovery-vmware-to-azure-manage-vCenter.md##delete-a-vcenter-in-azure-site-recovery) 中的步骤操作，删除对 vCenter 的引用。

4. 按照[解除配置服务器授权](site-recovery-vmware-to-azure-manage-configuration-server.md##decommissioning-a-configuration-server)中的步骤操作，删除配置服务器。

5. 删除保管库。

### <a name="hyper-v-vms-with-virtual-machine-manager-to-azure"></a>Hyper-V VM（带 Virtual Machine Manager）到 Azure
1. 按照[禁用 VMware VM 或物理服务器保护](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server)中的步骤操作，删除所有受保护的 VM。

2. 按照[删除复制策略](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy)中的步骤操作，删除所有复制策略。

3.  按照[取消注册已连接的 VMM 服务器](site-recovery-manage-registration-and-protection.md##unregister-a-connected-vmm-server)中的步骤操作，删除对 Virtual Machine Manager 服务器的引用。

4.  删除保管库。

### <a name="hyper-v-vms-without-virtual-machine-manager-to-azure"></a>Hyper-V VM（不带 Virtual Machine Manager）到 Azure
1. 按照[禁用 VMware VM 或物理服务器保护](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server)中的步骤操作，删除所有受保护的 VM。

2. 按照[删除复制策略](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy)中的步骤操作，删除所有复制策略。

3. 按照[取消注册 Hyper-V 主机](/site-recovery-manage-registration-and-protection.md##unregister-a-hyper-v-host-in-a-hyper-v-site)中的步骤操作，删除对 Hyper-V 服务器的引用。

4. 删除 Hyper-V 站点。

5. 删除保管库。

<!--Update_Description: add new feature of how to delete vault from VMWare to azure-->