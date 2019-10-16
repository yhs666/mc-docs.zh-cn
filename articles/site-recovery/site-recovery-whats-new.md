---
title: Azure Site Recovery 中的新增功能
description: 提供 Azure Site Recovery 中引入的新功能的摘要
services: site-recovery
author: rockboyfor
ms.service: site-recovery
ms.topic: conceptual
origin.date: 09/12/2019
ms.date: 09/30/2019
ms.author: v-yeche
ms.openlocfilehash: 7998437d8c44b6f31b88c6be08cb01a85ec95a09
ms.sourcegitcommit: 332ae4986f49c2e63bd781685dd3e0d49c696456
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340792"
---
# <a name="whats-new-in-site-recovery"></a>Site Recovery 中的新增功能

[Azure Site Recovery](site-recovery-overview.md) 服务会持续进行更新和改进。 本文介绍最新版本、新功能和新内容，让你始终了解最新动态。 此页会定期更新。

可以在 [Azure 更新](https://azure.microsoft.com/updates/?product=site-recovery)频道中关注并订阅 Site Recovery 更新通知。

<!--CORRECT ON [Azure updates](https://azure.microsoft.com/updates/?product=site-recovery)-->

## <a name="supported-updates"></a>支持的更新

对于 Site Recovery 组件，我们支持 N-4 版本，其中 N 是最新发布的版本。 下表汇总了这些更新。

**更新** |  **统一安装程序** | **配置服务器 ova** | **移动服务代理** | **Site Recovery 提供程序** | **恢复服务代理**
--- | --- | --- | --- | --- | ---
[汇总 40](https://support.microsoft.com/help/4517283/) | 9.28.5345.1 | 5.1.4800.0 | 9.28.5345.1 | 5.1.4800.0 | 2.0.9165.0
[汇总 39](https://support.microsoft.com/help/4517283/) | 9.27.5308.1 | 5.1.4600.0 | 9.27.5308.1 | 5.1.4600.0 | 2.0.9165.0
[汇总 38](https://support.microsoft.com/help/4513507/) | 9.26.5269.1 | 5.1.4500.0 | 9.26.5269.1 | 5.1.4500.0 | 2.0.9165.0
[汇总 37](https://support.microsoft.com/help/4508614/) | 9.25.5241.1 | 5.1.4300.0 | 9.25.5241.1 | 5.1.4300.0 | 2.0.9163.0
[汇总 36](https://support.microsoft.com/help/4503156/) | 9.24.5211.1 | 5.1.4150.0 | 9.24.5211.1 | 5.1.4150.0 | 2.0.9160.0 

[详细了解](service-updates-how-to.md)更新安装和支持。

## <a name="updates-september-2019"></a>更新（2019 年 9 月）

### <a name="update-rollup-40"></a>更新汇总 40

[更新汇总 40](https://support.microsoft.com/help/4521530/update-rollup-40-for-azure-site-recovery) 提供以下更新。

<!--MOONCAKE: CORRECT ON https://)

**Update** | **Details**
--- | ---
**Providers and agents** | Updates to Site Recovery agents and providers (as detailed in the rollup)
**Issue fixes/improvements** | A number of fixes and improvements (as detailed in the rollup)

### Azure VM disaster recovery

New features for Azure VM disaster recovery are summarized in the table.

**Feature** | **Details**
--- | ---
**Cleanup after failback** | After failing over to the secondary Azure, and then failing back to the primary region, Site Recovery automatically cleans up machines in the secondary region. There's no need to manually delete VMS and NICs.
**Test failover retains IP address** | You can now retain the IP address of the source VM during a disaster recovery drill, and pick a static IP address for a test failover.

### VMware/physical server disaster recovery

Features added this month are summarized in the table.

**Feature** | **Details**
--- | ---
New process server alerts | We've added new process server alerts. [Learn more](vmware-physical-azure-monitor-process-server.md). 

### Hyper-V disaster recovery

Features added this month are summarized in the table.

**Feature** | **Details**
--- | ---
Storage account | Site Recovery now supports the use of storage accounts with firewall enabled for Hyper-V to Azure disaster recovery.  You can select firewall-enabled storage accounts as a target account, or for cache storage. If you use firewall-enabled account, make sure that you enable the option to allow trusted Azure services.

## Updates (August 2019)

### Update rollup 39

[Update rollup 39](https://support.microsoft.com/help/4517283/update-rollup-39-for-azure-site-recovery) provides the following updates.

**Update** | **Details**
--- | ---
**Providers and agents** | Updates to Site Recovery agents and providers (as detailed in the rollup)
**Issue fixes/improvements** | A number of fixes and improvements (as detailed in the rollup)

### Azure VM disaster recovery

New features for Azure VM disaster recovery are summarized in the table.

**Feature** | **Details**
--- | ---
**Encryption without Azure AD** | Encryption without an Azure AD app is now supported for Azure VM replication to managed disks running Windows.
**Network resources for failover** | When failing over to another region, you can now attach network resource settings (NSGs, load balancing, public IP address) to a VM. 

## Updates (July 2019)

### Update rollup 38

[Update rollup 38](https://support.microsoft.com/help/4513507/) provides the following updates.

**Update** | **Details**
--- | ---
**Providers and agents** | Updates to Site Recovery agents and providers (as detailed in the rollup)
**Issue fixes/improvements** | A number of fixes and improvements (as detailed in the rollup)

### General

Site Recovery now supports used of general purpose v2 storage accounts for cache storage or target storage. Previously only v1 was supported.

### VMware to Azure disaster recovery

You can now replicate disks up to 8 TB, when replicating to an Azure VM with managed disks.

## Updates (June 2019)

### Update rollup 37

[Update rollup 37](https://support.microsoft.com/help/4508614/) provides the following updates.

**Update** | **Details**
--- | ---
**Providers and agents** | Updates to Site Recovery agents and providers (as detailed in the rollup)
**Issue fixes/improvements** | A number of fixes and improvements (as detailed in the rollup)

### VMware/physical server disaster recovery

Features added this month are summarized in the table.

**Feature** | **Details**
--- | ---
**GPT partitions** | From Update Rollup 37 onwards (Mobility service version 9.25.5241.1), up to five GPT partitions are supported in UEFI. Prior to this update, four were supported.

## Updates (May 2019)

### Update rollup 36

[Update rollup 36](https://support.microsoft.com/help/4503156) provides the following updates.

**Update** | **Details**
--- | ---
**Providers and agents** | An update to Site Recovery agents and providers (as detailed in the rollup)
**Issue fixes/improvements** | A number of fixes and improvements (as detailed in the rollup)

### Azure VM disaster recovery

Features added this month are summarized in the table.

**Feature** | **Details**
--- | ---
**Replicate added disks** | Enable replication for data disks added to an Azure VM that's already enabled for disaster recovery. [Learn more](azure-to-azure-enable-replication-added-disk.md).

<!--Not Available on **Automatic updates** | When configuring automatic updates for the Mobility service extension that runs on Azure VMs enabled for disaster recovery, you can now select an existing automation account to use, instead of using the default account created by Site Recovery.-->

<!--Not Available on  [Learn more](azure-to-azure-autoupdate.md)-->

### <a name="vmwarephysical-server-disaster-recovery"></a>VMware/物理服务器灾难恢复

下表中总结了本月添加的功能。

**功能** | **详细信息**
--- | ---
**进程服务器监视** | 对于本地 VMware VM 和物理服务器的灾难恢复，通过改进的服务器运行状况报告和警报来监视和排查进程服务器问题。 [了解详细信息](vmware-physical-azure-monitor-process-server.md)。 

## <a name="updates-march-2019"></a>更新（2019 年 3 月）

### <a name="update-rollup-35"></a>更新汇总 35

[更新汇总 35](https://support.microsoft.com/help/4494485/update-rollup-35-for-azure-site-recovery) 提供以下更新。

**更新** | **详细信息**
--- | ---
**提供程序和代理** | 对 Site Recovery 代理和提供程序的更新（参阅汇总中的详述）
**问题修复/改进** | 已做出多项修复和改进（参阅汇总中的详述）

### <a name="vmwarephysical-server-disaster-recovery"></a>VMware/物理服务器灾难恢复

下表中总结了本月添加的功能。

**功能** | **详细信息**
--- | ---
**托管磁盘** | 现在可将本地 VMware VM 和物理服务器直接复制到 Azure 中的托管磁盘。 本地数据将发送到 Azure 中的缓存存储帐户，恢复点将在目标位置的托管磁盘中创建。 这可确保无需管理多个目标存储帐户。
**配置服务器** | Site Recovery 现在支持具有多个 NIC 的配置服务器。 先将附加的适配器添加到配置服务器 VM，然后再在保管库中注册配置服务器。 如果在注册之后再添加，则需要在保管库中重新注册服务器。

## <a name="updates-february-2019"></a>更新（2019 年 2 月）

### <a name="update-rollup-34"></a>更新汇总 34 

[更新汇总 34](https://support.microsoft.com/help/4490016/update-rollup-34-for-azure-site-recovery) 提供以下更新。

**更新** | **详细信息**
--- | ---
**提供程序和代理** | 对 Site Recovery 代理和提供程序的更新（参阅汇总中的详述）。
**问题修复/改进** | 已做出多项修复和改进（参阅汇总中的详述）。

### <a name="update-rollup-33"></a>更新汇总 33 

[更新汇总 33](https://support.microsoft.com/help/4489582/update-rollup-33-for-azure-site-recovery) 提供以下更新。

**更新** | **详细信息**
--- | ---
**提供程序和代理** | 对 Site Recovery 代理和提供程序的更新（参阅汇总中的详述）。
**问题修复/改进** | 已做出多项修复和改进（参阅汇总中的详述）。

### <a name="azure-vm-disaster-recovery"></a>Azure VM 灾难恢复 
下表中总结了本月添加的功能。

**功能** | **详细信息**
--- | ---
**网络映射** | 对于 Azure VM 灾难恢复，现在可以在启用复制时使用任何可用的目标网络。 
**标准 SSD** | 现在可以使用[标准 SSD 磁盘](/virtual-machines/windows/disks-standard-ssd)对 Azure VM 设置灾难恢复。
**存储空间直通** | 可以使用[存储空间直通](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)对 Azure VM 应用中运行的应用设置灾难恢复，以实现高可用性。  将存储空间直通 (S2D) 与 Site Recovery 结合使用可为 Azure VM 工作负荷提供全面的保护。 使用 S2D 可在 Azure 中托管来宾群集。 当 VM 托管了关键应用程序（例如 SAP ASCS 层、SQL Server 或横向扩展文件服务器）时，此功能特别有用。

### <a name="vmwarephysical-server-disaster-recovery"></a>VMware/物理服务器灾难恢复
下表中总结了本月添加的功能。

**功能** | **详细信息**
--- | ---
**Linux BRTFS 文件系统** | Site Recovery 现在支持复制采用使用 BRTFS 文件系统的 VMware VM。 如果存在以下情况，则不支持复制：<br/><br/>- 启用复制后 BTRFS 文件系统子卷发生更改。<br/><br/>- 文件系统分散在多个磁盘中。<br/><br/>- BTRFS 文件系统支持 RAID。
**Windows Server 2019** | 添加了对运行 Windows Server 2019 的计算机的支持。

## <a name="updates-january-2019"></a>更新（2019 年 1 月）

### <a name="accelerated-networking-azure-vms"></a>加速网络 (Azure VM)

使用加速网络可以实现对 VM 的单根 I/O 虚拟化 (SR-IOV)，提升网络性能。 为 Azure VM 启用复制时，Site Recovery 会检测是否启用了加速网络。 如果启用了加速网络，Site Recovery 会在故障转移后自动在目标副本 Azure VM 上针对 [Windows](/virtual-network/create-vm-accelerated-networking-powershell#enable-accelerated-networking-on-existing-vms) 和 [Linux](/virtual-network/create-vm-accelerated-networking-cli#enable-accelerated-networking-on-existing-vms) 配置加速网络。

[了解详细信息](azure-vm-disaster-recovery-with-accelerated-networking.md)。

### <a name="update-rollup-32"></a>更新汇总 32 

[更新汇总 32](https://support.microsoft.com/help/4485985/update-rollup-32-for-azure-site-recovery) 提供以下更新。

**更新** | **详细信息**
--- | ---
**提供程序和代理** | 对 Site Recovery 代理和提供程序的更新（参阅汇总中的详述）。
**问题修复/改进** | 已做出多项修复和改进（参阅汇总中的详述）。

### <a name="azure-vm-disaster-recovery"></a>Azure VM 灾难恢复

下表中总结了本月添加的功能。

**功能** | **详细信息**
--- | ---
**Linux 支持** | 添加了对 Ubuntu、Debian 和 SUSE 新内核版本的支持。
**存储空间直通** | Site Recovery 支持使用存储空间直通 (S2D) 的 Azure VM。

<!--Not Available on RedHat Workstation 6/7-->

### <a name="vmware-vmsphysical-servers-disaster-recovery"></a>VMware VM/物理服务器灾难恢复

下表中总结了本月添加的功能。

**功能** | **详细信息**
--- | ---
**Linux 支持** | 添加了对 Ubuntu、Debian 和 SUSE 新内核版本的支持。

<!--Not Available on Redhat Enterprise Linux 7.6, RedHat Workstation 6/7, Oracle Linux 6.10/7.6-->

### <a name="update-rollup-31"></a>更新汇总 31 

[更新汇总 31](https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery) 提供以下更新。

**更新** | **详细信息**
--- | ---
**提供程序和代理** | 对 Site Recovery 代理和提供程序的更新（参阅汇总中的详述）。
**问题修复/改进** | 已做出多项修复和改进（参阅汇总中的详述）。

### <a name="vmware-vmsphysical-servers-replication"></a>VMware VM/物理服务器复制 
下表中总结了本月添加的功能。
**功能** | **详细信息**
--- | ---
**Linux 支持** | 添加了对 Oracle Linux 6.8 和 6.9/7.0 以及 UEK5 内核的支持。
**LVM** | 添加了对 LVM 和 LVM2 卷的支持。<br/><br/> 现在支持磁盘分区和 LVM 卷上的 /boot 目录。
**Directories** | 添加了对这些设置为独立分区的目录，或不在同一系统磁盘中的文件系统的支持：<br/><br/> /(root)、/boot、/usr、/usr/local、/var、/etc。
**Windows Server 2008** | 添加了对动态磁盘的支持。
**故障转移** | 改进了 storvsc 和 vsbus 不是启动驱动程序的 VMware VM 的故障转移时间。
**UEFI 支持** | Azure VM 不支持 UEFI 启动类型。 现在可以通过 Site Recovery 将使用 UEFI 的本地物理服务器迁移到 Azure。 Site Recovery 在迁移服务器之前会将启动类型转换为 BIOS。 Site Recovery 以前仅支持对 VM 执行此转换。 该项支持适用于运行 Windows Server 2012 或更高版本的物理服务器。

### <a name="azure-vm-disaster-recovery"></a>Azure VM 灾难恢复
下表中总结了本月添加的功能。

**功能** | **详细信息**
--- | ---
**Linux 支持** | 添加了对 Oracle Linux 6.8 和 6.9/7.0 以及 UEK5 内核的支持。
**Linux BRTFS 文件系统** | 支持对 Azure VM 使用。
**已启用防火墙的存储（门户/PowerShell）** | 添加了对[已启用防火墙的存储帐户](/storage/common/storage-network-security)的支持。<br/><br/> 可将已启用防火墙的存储帐户中使用非托管磁盘的 Azure VM 复制到另一个 Azure 区域，以实现灾难恢复。<br/><br/> 可将已启用防火墙的存储帐户用作非托管磁盘的目标存储帐户。<br/><br/> 支持在门户和 PowerShell 中使用。

<!--Not Available on **Azure VMs in availability zones** | You can enable replication to another region for Azure VMs deployed in availability zones. You can now enable replication for an Azure VM, and set the target for failover to a single VM instance, a VM in an availability set, or a VM in an availability zone. The setting doesn't impact replication. [Read](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region/) the announcement.-->

## <a name="updates-december-2018"></a>更新（2018 年 12 月）

### <a name="automatic-updates-for-the-mobility-service-azure-vms"></a>移动服务的自动更新 (Azure VM)

Site Recovery 增加了一个选项，可以针对移动服务扩展进行自动更新。 移动服务扩展安装在每个通过 Site Recovery 复制的 Azure VM 上。 启用复制时，请选择是否允许 Site Recovery 管理对扩展的更新。

更新不需 VM 重启，且不影响复制。

<!--Not Available on  [Learn more](azure-to-azure-autoupdate.md)-->

### <a name="pricing-calculator-for-azure-vm-disaster-recovery"></a>针对 Azure VM 灾难恢复的价格计算器

进行 Azure VM 的灾难恢复需支付 VM 许可费用以及网络和存储费用。 Azure 提供[定价计算器](https://www.azure.cn/zh-cn/pricing/calculator/)，用于计算这些费用。 Site Recovery 现在提供[定价估算](https://www.azure.cn/zh-cn/pricing/calculator/)功能，可以根据三层应用的情况对示例部署定价。该应用使用 6 个 VM，其中包含 12 个标准 HDD 磁盘和 6 个高级 SSD 磁盘。

- 此示例假设标准磁盘的数据更改率为每天 10 GB，高级磁盘的数据更改率为每天 20 GB。
- 对于特定的部署，可以更改变量来估算费用。
- 可以指定 VM 的数目、托管磁盘的数目和类型，以及预期的跨 VM 总数据更改率。
- 另外，可以通过应用一个压缩因素来估算带宽费用。

[阅读](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/)公告。

## <a name="updates-october-2018"></a>更新（2018 年 10 月）

### <a name="update-rollup-30"></a>更新汇总 30 

[更新汇总 30](https://support.microsoft.com/help/4468181/azure-site-recovery-update-rollup-30) 提供以下更新。

**更新** | **详细信息**
--- | ---
**提供程序和代理** | 对 Site Recovery 代理和提供程序的更新（参阅汇总中的详述）。
**问题修复/改进** | 已做出多项修复和改进（参阅汇总中的详述）。

### <a name="azure-vm-disaster-recovery"></a>Azure VM 灾难恢复
下表中总结了本月添加的功能。

**功能** | **详细信息**
--- | ---
**磁盘加密支持** | 添加了对在 Azure AD 应用中使用 Azure 磁盘加密 (ADE) 进行加密的 Azure VM 进行灾难恢复的支持。
**磁盘排除** | 现在，在 Azure VM 复制期间会自动排除未初始化的磁盘。
**已启用防火墙的存储 (PowerShell)** | 添加了对[已启用防火墙的存储帐户](/storage/common/storage-network-security)的支持。<br/><br/> 可将已启用防火墙的存储帐户中使用非托管磁盘的 Azure VM 复制到另一个 Azure 区域，以实现灾难恢复。<br/><br/> 可将已启用防火墙的存储帐户用作非托管磁盘的目标存储帐户。<br/><br/> 仅支持在 PowerShell 中使用。

<!--Not Available on **Region support** | Site Recovery support added for Austrial Central 1 and Austrial Central 2.-->
<!--Not Available on  [了解详细信息](azure-to-azure-how-to-enable-replication-ade-vms.md)-->

### <a name="update-rollup-29"></a>更新汇总 29 

[更新汇总 29](https://support.microsoft.com/help/4466466/update-rollup-29-for-azure-site-recovery) 提供以下更新。

**更新** | **详细信息**
--- | ---
**提供程序和代理** | 对 Site Recovery 代理和提供程序的更新（参阅汇总中的详述）。
**问题修复/改进** | 已做出多项修复和改进（参阅汇总中的详述）。

## <a name="updates-august-2018"></a>更新（2018 年 8 月）

### <a name="update-rollup-28"></a>更新汇总 28 

[更新汇总 28](https://support.microsoft.com/help/4460079/update-rollup-28-for-azure-site-recovery) 提供以下更新。

**更新** | **详细信息**
--- | ---
**提供程序和代理** | 对 Site Recovery 代理和提供程序的更新（参阅汇总中的详述）。
**问题修复/改进** | 已做出多项修复和改进（参阅汇总中的详述）。

### <a name="azure-vm-disaster-recovery"></a>Azure VM 灾难恢复 
下表中总结了本月添加的功能。

**功能** | **详细信息**
--- | ---
**Linux 支持** | 添加了对 CentOS 6.10 的支持。<br/><br/>
**跨订阅灾难恢复** | 支持将一个区域中的 Azure VM 复制到同一 Azure Active Directory 租户中不同订阅内的另一个区域。 [了解详细信息](https://aka.ms/cross-sub-blog)。

<!--Not Available on **Cloud support** | Supported disaster recovery for Azure VMs in the Germany cloud.-->
<!--Not Available on RedHat Enterprise Linux 6.10;-->

### <a name="vmware-vmphysical-server-disaster-recovery"></a>VMware VM/物理服务器灾难恢复 
下表中总结了本月添加的功能。

**功能** | **详细信息**
--- | ---
**Linux 支持** | 添加了对 CentOS 6.10 的支持。<br/><br/> 现在支持基于 Linux 且在旧式 BIOS 兼容模式下使用 GUID 分区表 (GPT) 分区样式的 VM。 有关详细信息，请查看 [Azure VM 常见问题解答](/virtual-machines/linux/faq-for-disks)。 
**迁移后的 VM 灾难恢复** | 支持将已迁移到 Azure 的本地 VMware VM 灾难恢复到次要区域，启用复制之前无需在 VM 上卸载移动服务。
**Windows Server 2008** | 支持迁移运行 Windows Server 2008 R2/2008 64 位和 32 位的计算机。<br/><br/> 仅限迁移（复制和故障转移）。 不支持故障回复。

<!--Not Available on RedHat Enterprise Linux 6.10,-->

## <a name="updates-july-2018"></a>更新（2018 年 7 月）

### <a name="update-rollup-27-july-2018"></a>更新汇总 27（2018 年 7 月）

[更新汇总 27](https://support.microsoft.com/help/4055712/update-rollup-27-for-azure-site-recovery) 提供以下更新。

**更新** | **详细信息**
--- | ---
**提供程序和代理** | 对 Site Recovery 代理和提供程序的更新（参阅汇总中的详述）。
**问题修复/改进** | 已做出多项修复和改进（参阅汇总中的详述）。

### <a name="azure-vm-disaster-recovery"></a>Azure VM 灾难恢复 

下表中总结了本月添加的功能。

**功能** | **详细信息**
--- | ---
**Linux 支持** | 添加了对 Red Hat Enterprise Linux 7.5 的支持。

### <a name="vmware-vmphysical-server-disaster-recovery"></a>VMware VM/物理服务器灾难恢复 

下表中总结了本月添加的功能。

**功能** | **详细信息**
--- | ---
**Linux 支持** | 添加了对 Red Hat Enterprise Linux 7.5、SUSE Linux Enterprise Server 12 的支持。

## <a name="next-steps"></a>后续步骤

在 [Azure 更新](https://www.azure.cn/what-is-new/)页上时刻了解更新。

<!--Update_Description: wording update -->
