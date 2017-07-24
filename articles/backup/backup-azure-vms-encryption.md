---
title: "使用 Azure 备份来备份和还原加密型 VM"
description: "本文介绍如何备份和还原使用 Azure 磁盘加密进行加密的 VM。"
services: backup
documentationcenter: 
author: alexchen2016
manager: digimobile
editor: 
ms.assetid: 8387f186-7d7b-400a-8fc3-88a85403ea63
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
origin.date: 04/24/2017
ms.date: 06/29/2017
ms.author: v-junlch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b520a061c5a5aa699a74052909a9e223596193a1
ms.sourcegitcommit: d5d647d33dba99fabd3a6232d9de0dacb0b57e8f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2017
---
# <a name="how-to-back-up-and-restore-encrypted-virtual-machines-with-azure-backup"></a>如何通过 Azure 备份来备份和还原加密的虚拟机
本文介绍使用 Azure 备份来备份和还原虚拟机的步骤， 同时提供有关支持的方案、先决条件和错误案例故障排除步骤的详细信息。

## <a name="supported-scenarios"></a>支持的方案
> [!NOTE]
> 1. 仅 Resource Manager 部署型虚拟机支持备份和还原加密型 VM。 经典部署型虚拟机不支持此功能。 <br>
> 2. 使用 Azure 磁盘加密的 Windows 和 Linux 虚拟机支持此功能，Azure 磁盘加密利用 Windows 的行业标准 BitLocker 功能和 Linux 的 DM-Crypt 功能加密磁盘。 <br>
> 3. 仅同时使用 BitLocker 加密密钥和密钥加密密钥进行加密的虚拟机支持此功能。 仅使用 BitLocker 加密密钥进行加密的虚拟机不支持此功能。 <br>
>
>

## <a name="pre-requisites"></a>先决条件
1. 虚拟机已使用 Azure 磁盘加密进行加密。 虚拟机应同时使用 BitLocker 加密密钥和密钥加密密钥进行加密。
2. 已创建恢复服务保管库并已按照[为备份做环境准备](backup-azure-arm-vms-prepare.md)一文中所提到的步骤设置存储复制。

## <a name="restore-encrypted-vm"></a>还原加密型 VM
若要还原已加密的 VM，请先使用“选择 VM 还原配置”的 **还原已备份磁盘** 部分中提到的步骤还原磁盘。 之后，可以使用以下选项之一：
- 使用[从还原的磁盘创建 VM](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) 中提到的 PowerShell 步骤从还原的磁盘创建完整的 VM。
- 或者，使用在还原磁盘过程中生成的模板从还原的磁盘创建 VM。 仅对于 2017 年 4 月 26 日以后创建的恢复点，可以使用模板。

## <a name="troubleshooting-errors"></a>排查错误
| 操作 | 错误详细信息 | 解决方法 |
| --- | --- | --- |
| 备份 |验证失败，因为虚拟机仅使用 BEK 进行加密。 仅可为同时使用 BEK 和 KEK 进行加密的虚拟机启用备份。 |虚拟机应使用 BEK 和 KEK 进行加密。 首先解密 VM，然后使用 BEK 和 KEK 对其进行加密。 使用 BEK 和 KEK 对 VM 进行加密后，即可启用备份。  |
| 还原 |无法还原此加密型 VM，因为与此 VM 关联的密钥保管库不存在。 |按照 [Azure Key Vault 入门](../key-vault/key-vault-get-started.md)中的步骤创建 Key Vault。 请参阅[使用 Azure 备份还原 Key Vault 密钥和机密](backup-azure-restore-key-secret.md)一文，还原密钥和机密（如果它们不存在）。 |
| 还原 |无法还原此加密型 VM，因为与此 VM 关联的密钥和机密不存在。 |请参阅[使用 Azure 备份还原 Key Vault 密钥和机密](backup-azure-restore-key-secret.md)一文，还原密钥和机密（如果它们不存在）。 |
| 还原 |备份服务无权访问用户订阅中的资源。 |如上所述，先使用“选择 VM 还原配置”的 **还原已备份磁盘** 部分中提到的步骤还原磁盘。 之后，使用 PowerShell [从还原的磁盘创建 VM](backup-azure-vms-automation.md#create-a-vm-from-restored-disks)。 |
|备份 | Azure 备份服务对 Key Vault 没有足够的权限，无法备份加密的虚拟机 | 虚拟机应同时使用 BitLocker 加密密钥和密钥加密密钥进行加密。 之后，应启用备份。  使用[使用 AzureRM.RecoveryServices.Backup cmdlet 来备份虚拟机](backup-azure-vms-automation.md)中的 PowerShell 文档的“启用保护”部分提到的步骤，可在 PowerShell 中为备份服务提供这些权限。 |  

