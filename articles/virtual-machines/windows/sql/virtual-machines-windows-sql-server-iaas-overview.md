---
title: "Azure 虚拟机中的 SQL Server 概述 | Azure"
description: "了解如何在 Azure 虚拟机上运行完整的 SQL Server 版本。 获取所有 SQL Server VM 映像和相关内容的直接链接。"
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: c505089e-6bbf-4d14-af0e-dd39a1872767
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
origin.date: 01/09/2017
ms.date: 07/03/2017
ms.author: v-dazen
ms.openlocfilehash: cb2dda90eddb782c7da0d2edf108a28b7884ee0a
ms.sourcegitcommit: 54fcef447f85b641d5da65dfe7016f87e29b40fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2017
---
# Azure 虚拟机中的 SQL Server 概述
<a id="overview-of-sql-server-on-azure-virtual-machines" class="xliff"></a>
本主题介绍了在 Azure 虚拟机 (VM) 上运行 SQL Server 的选项，提供了[门户映像链接](#option-1-create-a-sql-vm-with-per-minute-licensing)，同时概述了[常见任务](#manage-your-sql-vm)。

> [!NOTE]
> 如果已熟悉 SQL Server，只是想了解如何部署 SQL Server VM，请参阅[在 Azure 门户中预配 SQL Server 虚拟机](virtual-machines-windows-portal-sql-server-provision.md)。
> 
> 

## 概述
<a id="overview" class="xliff"></a>
如果是数据库管理员或开发人员，Azure 虚拟机能够将本地 SQL Server 工作负荷和应用程序迁移到云。 下面的视频是关于 SQL Server Azure VM 的技术概述。

## 方案
<a id="scenarios" class="xliff"></a>
选择用在 Azure 中托管数据有诸多理由。 将应用程序移到 Azure 后，移动数据的性能也有改善。 而且还有其他方面的好处。 会自动获取多个数据中心的访问权限，从而获得全局支持和灾难恢复能力。 并且数据持久保存、高度安全。

可用使用“在 Azure VM 中运行 SQL Server”选项在 Azure 中存储关系数据。 它对于几个方案来说是不错的选择。 例如，你可能想要配置与本地 SQL Server 计算机高度相似的 Azure VM。 或者可能想要在同一数据库服务器上运行其他应用程序和服务。 有两个主要资源，可帮助用户仔细考虑更多方案和注意事项：

* [虚拟机上的 SQL Server](https://www.azure.cn/home/features/virtual-machines/#home_vm_overview_info) 概述了在 Azure VM 中使用 SQL Server 的最佳方案。 
* [选择云 SQL Server 选项：Azure VM (IaaS) 中的 Azure SQL (PaaS) 数据库或 SQL Server](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md) 详细比较了在 VM 中运行的 SQL 数据库和 SQL Server。

## 创建新的 SQL VM
<a id="create-a-new-sql-vm" class="xliff"></a>
下面的各部分针对相关 SQL Server 虚拟机库映像提供了指向 Azure 门户的直接链接。

在以下教程中查找创建新 SQL VM 的分步指南：[在 Azure 门户中预配 SQL Server 虚拟机](virtual-machines-windows-portal-sql-server-provision.md)。 另请查看 [Performance best practices for SQL Server VMs](virtual-machines-windows-sql-performance.md)（SQL Server VM 的性能最佳实践），该文介绍了如何在预配期间选择适当的虚拟机大小和其他可用功能。

## 选项 1：使用每分钟许可创建 SQL VM
<a id="option-1-create-a-sql-vm-with-per-minute-licensing" class="xliff"></a>
下表提供了虚拟机库中提供的最新 SQL Server 映像的矩阵。 单击任何链接，即可开始创建具有指定版本和操作系统的新 SQL VM。 

> [!TIP]
> 若要了解这些映像的 VM 和 SQL 定价，请参阅 [SQL Server Azure VM 的定价指南](virtual-machines-windows-sql-server-pricing-guidance.md)。

| 版本 | 操作系统 | 版本 |
| --- | --- | --- |
| **SQL Server 2016 SP1** |Windows Server 2016 |[Enterprise](https://portal.azure.cn/#create/Microsoft.SQLServer2016SP1EnterpriseWindowsServer2016)、[Standard](https://portal.azure.cn/#create/Microsoft.SQLServer2016SP1StandardWindowsServer2016)、[Web](https://portal.azure.cn/#create/Microsoft.SQLServer2016SP1WebWindowsServer2016)、[Express](https://portal.azure.cn/#create/Microsoft.SQLServer2016SP1ExpressWindowsServer2016)、[Developer](https://portal.azure.cn/#create/Microsoft.SQLServer2016SP1DeveloperWindowsServer2016) |
| **SQL Server 2014 SP2** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.cn/#create/Microsoft.SQLServer2014SP2EnterpriseWindowsServer2012R2)、[Standard](https://portal.azure.cn/#create/Microsoft.SQLServer2014SP2StandardWindowsServer2012R2)、[Web](https://portal.azure.cn/#create/Microsoft.SQLServer2014SP2WebWindowsServer2012R2)、[Express](https://portal.azure.cn/#create/Microsoft.SQLServer2014SP2ExpressWindowsServer2012R2) |
| **SQL Server 2012 SP3** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.cn/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2)、[Standard](https://portal.azure.cn/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2)、[Web](https://portal.azure.cn/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2)、[Express](https://portal.azure.cn/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2) |
| **SQL Server 2008 R2 SP3** |Windows Server 2008 R2 |[Enterprise](https://portal.azure.cn/#create/Microsoft.SQLServer2008R2SP3EnterpriseWindowsServer2008R2)、[Standard](https://portal.azure.cn/#create/Microsoft.SQLServer2008R2SP3StandardWindowsServer2008R2)、[Web](https://portal.azure.cn/#create/Microsoft.SQLServer2008R2SP3WebWindowsServer2008R2) |

除了此列表，也可以使用 SQL Server 版本和操作系统的其他组合。 在 Azure 门户中通过应用商店搜索查找其他映像。 

## <a id="BYOL"></a> 选项 2：使用现有许可创建 SQL VM
你也可以自带许可 (BYOL)。 在此方案中，你只需支付 VM 费用，SQL Server 许可不需要任何额外的费用。 若要使用自己的许可证，请参考下面的 SQL Server 版本和操作系统矩阵。 在门户中，这些映像名称带有 **{BYOL}**前缀。

> [!TIP]
> 自带许可证长时间会节省资金，因为可以持续使用生产型工作负荷。 有关详细信息，请参阅 [SQL Server Azure VM 定价指南](virtual-machines-windows-sql-server-pricing-guidance.md)。

| 版本 | 操作系统 | 版本 |
| --- | --- | --- |
| **SQL Server 2016 SP1** |Windows Server 2016 |[Enterprise BYOL](https://portal.azure.cn/#create/Microsoft.BYOLSQLServer2016SP1StandardWindowsServer2016)、[Standard BYOL](https://portal.azure.cn/#create/Microsoft.BYOLSQLServer2016SP1StandardWindowsServer2016) |
| **SQL Server 2014 SP2** |Windows Server 2012 R2 |[Enterprise BYOL](https://portal.azure.cn/#create/Microsoft.BYOLSQLServer2014SP2EnterpriseWindowsServer2012R2)、[Standard BYOL](https://portal.azure.cn/#create/Microsoft.BYOLSQLServer2014SP2StandardWindowsServer2012R2) |
| **SQL Server 2012 SP2** |Windows Server 2012 R2 |[Enterprise BYOL](https://portal.azure.cn/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2)、[Standard BYOL](https://portal.azure.cn/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2) |

除了此列表，也可以使用 SQL Server 版本和操作系统的其他组合。 在 Azure 门户中通过应用商店搜索查找其他映像（搜索“{BYOL} SQL Server”）。

> [!IMPORTANT]
> 若要使用 BYOL VM 映像，必须具有包含 [Azure 上通过软件保障实现的许可移动性](https://www.azure.cn/pricing/license-mobility/)的企业协议。 此外，还需要有所要使用的 SQL Server 版本的有效许可证。 必须在预配 VM 的 [10](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf) 天内**向 Microsoft 提供必要的 BYOL 信息**。 

> [!NOTE]
> 无法更改按分钟付费的 SQL Server VM 的许可模式来使用自己的许可证。 若要使用自己的许可证，必须创建新的 BYOL VM，并将数据库迁移到新 VM。 

## 管理 SQL VM
<a id="manage-your-sql-vm" class="xliff"></a>
预配 SQL Server VM 之后，有几项可选的管理任务。 在许多方面，完全可以像管理本地 SQL Server 实例一样配置和管理 SQL Server。 但某些任务是特定于 Azure 的。 下列各节重点介绍上述某些领域并提供详细信息链接。

### 连接到 VM
<a id="connect-to-the-vm" class="xliff"></a>
最基本的管理步骤之一是，通过 SQL Server Management Studio (SSMS) 之类的工具连接到 SQL Server VM。 有关如何连接到新 SQL Server VM 的说明，请参阅[连接到 Azure 上的 SQL Server 虚拟机](virtual-machines-windows-sql-connect.md)。

### 迁移数据
<a id="migrate-your-data" class="xliff"></a>
如果已有数据库，你会想要将该数据库移至新预配的 SQL VM。 有关迁移选项的列表和指导，请参阅[将数据库迁移到 Azure VM 上的 SQL Server](virtual-machines-windows-migrate-sql.md)。

### 配置高可用性
<a id="configure-high-availability" class="xliff"></a>
如果你需要高可用性，请考虑配置 SQL Server 可用性组。 这涉及虚拟网络中的多个 Azure VM。 如果想要手动配置可用性组和关联的侦听器，请参阅[在 Azure VM 中配置 AlwaysOn 可用性组](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)。

有关其他高可用性注意事项，请参阅 [Azure 虚拟机中 SQL Server 的高可用性和灾难恢复](virtual-machines-windows-sql-high-availability-dr.md)。

### 备份数据
<a id="back-up-your-data" class="xliff"></a>
Azure VM 可以利用[自动备份](virtual-machines-windows-sql-automated-backup.md)，这将定期在 Blob 存储中创建数据库的备份。 你也可以手动使用此技术。 有关详细信息，请参阅[使用 Azure 存储进行 SQL Server 备份和还原](virtual-machines-windows-use-storage-sql-server-backup-restore.md)。 有关所有备份和还原选项的概述，请参阅 [Azure 虚拟机中 SQL Server 的备份和还原](virtual-machines-windows-sql-backup-recovery.md)。

### 自动更新
<a id="automate-updates" class="xliff"></a>
Azure VM 可以使用[自动修补](virtual-machines-windows-sql-automated-patching.md)来安排维护时段，以便自动安装重要的 Windows 和 SQL Server 更新。

### 客户体验改善计划 (CEIP)
<a id="customer-experience-improvement-program-ceip" class="xliff"></a>
客户体验改善计划 (CEIP) 默认情况下已启用。 它定期将报表发送给 Microsoft，以帮助改进 SQL Server。 CEIP 不要求管理任务，除非想在预配后禁用它。 你可以通过远程桌面连接到 VM，以自定义或禁用 CEIP。 然后运行 **SQL Server 错误和使用情况报告** 实用工具。 请按照说明禁用报告功能。 

有关详细信息，请参阅[接受许可条款](https://msdn.microsoft.com/library/ms143343.aspx)主题的“CEIP”部分。 

## 后续步骤
<a id="next-steps" class="xliff"></a>

有关定价的问题，请参阅 [SQL Server Azure VM 的定价指南](virtual-machines-windows-sql-server-pricing-guidance.md)和 [Azure 定价页](https://www.azure.cn/pricing/details/virtual-machines/windows/)。 在“OS/软件”列表中选择 SQL Server 的目标版本。 然后查看不同大小的虚拟机的价格。

其他问题？ 请先参阅 [Azure 虚拟机中的 SQL Server 常见问题解答](virtual-machines-windows-sql-server-iaas-faq.md)。 同时将问题或看法添加到任何 SQL VM 主题的底部，以便与 Azure.cn 和社区互动。
