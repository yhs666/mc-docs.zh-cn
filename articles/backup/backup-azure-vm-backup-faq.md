---
title: Azure VM 备份常见问题解答
description: 针对下述常见问题的解答：Azure VM 备份原理、限制以及更改策略时会发生什么情况
services: backup
author: lingliw
manager: digimobile
ms.service: backup
ms.topic: conceptual
ms.date: 01/21/19
ms.author: v-lingwu
ms.openlocfilehash: 83f0611180bfd9e3ea5e6f96953fba27161ea5ca
ms.sourcegitcommit: c01292a935bd307a3326e86cb454d8fa2b561399
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/17/2019
ms.locfileid: "54363638"
---
# <a name="frequently-asked-questions-azure-backup"></a>常见问题解答 - Azure 备份

本文解答有关 [Azure 备份](backup-introduction-to-azure-backup.md)服务的常见问题。

## <a name="general-questions"></a>一般问题


### <a name="what-azure-vms-can-you-back-up-using-azure-backup"></a>使用 Azure 备份可以备份哪些 Azure VM？
请[查看](backup-azure-arm-vms-prepare.md#before-you-start)支持的操作系统和限制。



## <a name="backup"></a>Backup

### <a name="does-an-on-demand-backup-job-use-the-same-retention-schedule-as-scheduled-backups"></a>按需备份作业是否与计划的备份使用相同的保留计划？
否。 应为按需备份作业指定保留期。 默认情况下，从门户触发时，该作业会保留 30 天。

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-to-work"></a>我最近在一些 VM 上启用了 Azure 磁盘加密。 我的备份是否继续有效？
需要提供 Azure 备份访问 Key Vault 所需的权限。 请根据 [Azure 备份 PowerShell](backup-azure-vms-automation.md) 文档的“启用备份”部分中所述，在 PowerShell 中指定权限。

### <a name="i-migrated-vm-disks-to-managed-disks-will-my-backups-continue-to-work"></a>我将 VM 磁盘迁移到了托管磁盘。 我的备份是否继续有效？
是的，备份可以顺利工作。 无需重新配置任何设置。

### <a name="why-cant-i-see-my-vm-in-the-configure-backup-wizard"></a>为何在配置备份向导中看不到我的 VM？
该向导只会列出保管库所在的同一区域中的 VM，以及不在备份的 VM。


### <a name="my-vm-is-shut-down-will-an-on-demand-or-a-scheduled-backup-work"></a>我的 VM 已关闭。 按需备份或计划备份会运行吗？
是的。 计算机关闭时，将运行备份。 恢复点标记为崩溃一致。

### <a name="can-i-cancel-an-in-progress-backup-job"></a>可以取消正在进行的备份作业吗？
是的。 可以取消处于“正在创建快照”状态的备份作业。 如果作业正在从快照传输数据，则不能将它取消。

### <a name="i-enabled-resource-group-lock-on-my-backed-up-managed-disk-vms-will-my-backups-continue-to-work"></a>我在备份的托管磁盘 VM 上启用了资源组锁定。 我的备份是否继续有效？
如果锁定资源组，Azure 备份服务将无法删除较早的还原点。
- 由于还原点的最大数目限制为 18 个，因此新的备份会开始失败。
- 如果锁定后备份失败并显示内部错误，请[遵循这些步骤](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#clean-up-restore-point-collection-from-azure-portal)删除还原点集合。

### <a name="does-the-backup-policy-consider-daylight-saving-time-dst"></a>备份策略是否考虑夏令时 (DST)？
否。 本地计算机上的日期和时间是应用了当前夏令时的当地时间。 由于 DST，为计划的备份设置的时间可能不同于当地时间。


### <a name="does-azure-backup-support-standard-ssd-managed-disk"></a>Azure 备份是否支持标准 SSD 托管磁盘？
Azure 备份支持[标准 SSD 托管磁盘](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/)，该磁盘是 Azure 虚拟机的一种新型持久存储器。

## <a name="restore"></a>还原

### <a name="how-do-i-decide-whether-to-restore-disks-only-or-a-full-vm"></a>如何确定是仅还原磁盘还是要还原整个 VM？
可将 VM 还原视为 Azure VM 的快速创建选项。 此选项会更改磁盘名称、磁盘使用的容器、公共 IP 地址和网络接口名称。 创建 VM 时，会保留独特的资源。 VM 不会添加到可用性集。

如果有以下需求，请使用还原磁盘选项：
  * 自定义创建的 VM。 例如，更改大小。
  * 添加备份时不存在的配置设置
  * 控制所创建的资源的命名约定。
  * 将 VM 添加到可用性集。
  * 添加必须使用 PowerShell 或模板进行配置的其他任何设置。  w

### <a name="can-i-restore-backups-of-unmanaged-vm-disks-after-i-upgrade-to-managed-disks"></a>升级到托管磁盘后，是否可以还原非托管 VM 磁盘的备份？
是的，可以使用从非托管磁盘迁移到托管磁盘之前创建的备份。
- 默认情况下，还原 VM 作业将创建非托管 VM。
- 但是，你可以还原磁盘并使用这些磁盘来创建托管 VM。

### <a name="how-do-i-restore-a-vm-to-a-restore-point-before-the-vm-was-migrated-to-managed-disks"></a>如何将 VM 还原至将它迁移到托管磁盘之前的某个还原点？
默认情况下，还原 VM 作业将使用非托管磁盘创建 VM。 若要使用托管磁盘创建 VM，请执行以下操作：
1. [还原到非托管磁盘](tutorial-restore-disk.md#restore-a-vm-disk)。
2. [将还原的磁盘转换为托管磁盘](tutorial-restore-disk.md#convert-the-restored-disk-to-a-managed-disk)。
3. [使用托管磁盘创建 VM](tutorial-restore-disk.md#create-a-vm-from-the-restored-disk)。

[详细了解](backup-azure-vms-automation.md#restore-an-azure-vm)如何在 PowerShell 中执行此操作。

### <a name="can-i-restore-the-vm-thats-been-deleted"></a>是否可以还原已删除的 VM？
是的。 即使删除了 VM，也仍可以转到保管库中的相应备份项，然后从恢复点还原。

### <a name="how-to-restore-a-vm-to-the-same-availability-sets"></a>如何将 VM 还原到相同的可用性集？
对于托管磁盘 Azure VM，可以在还原托管磁盘时通过在模板中提供一个选项，来实现还原到可用性集。 此模板包含名为“可用性集”的输入参数。


## <a name="manage-vm-backups"></a>管理 VM 备份

### <a name="what-happens-if-i-modify-a-backup-policy"></a>如果修改备份策略，会发生什么情况？
VM 是使用已修改策略或新策略中的计划和保留设置备份的。

- 如果延长保留期，则会根据新策略对现有的恢复点进行标记和保留。
- 如果缩短保留期，则会将恢复点标记为在下一清理作业中删除，随后会将其删除。

### <a name="how-do-i-move-a-vm-backed-up-by-azure-backup-to-a-different-resource-group"></a>如何将 Azure 备份备份的 VM 转移到不同的资源组？

1. 暂时停止备份并保留备份数据。
2. 将 VM 移到目标资源组。
3. 在相同或新的保管库中重新启用备份。

可以从执行移动操作之前创建的可用还原点还原 VM。


<!--Update_Description: wording update -->
