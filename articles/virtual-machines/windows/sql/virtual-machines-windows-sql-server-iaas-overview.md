---
title: "Azure Windows 虚拟机上的 SQL Server 概述 | Azure"
description: "了解如何在 Azure 虚拟机上运行完整版本的 SQL Server。"
services: virtual-machines-windows
documentationcenter: 
author: rockboyfor
manager: digimobile
tags: azure-service-management
ms.assetid: c505089e-6bbf-4d14-af0e-dd39a1872767
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
origin.date: 12/14/2017
ms.date: 01/08/2018
ms.author: v-yeche
ms.openlocfilehash: 6bb7cd02f7739dcf26c486206dd7f6a224cb5154
ms.sourcegitcommit: 3629fd4a81f66a7d87a4daa00471042d1f79c8bb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2018
---
# <a name="what-is-sql-server-on-azure-virtual-machines-windows"></a>Azure 虚拟机上的 SQL Server 是什么？ (Windows)

> [!div class="op_single_selector"]
> * [Windows](virtual-machines-windows-sql-server-iaas-overview.md)
<!-- Not Available on > * [Linux](../../linux/sql/sql-server-linux-virtual-machines-overview.md) -->

[Azure 虚拟机上的 SQL Server](https://www.azure.cn/home/features/virtual-machines/#virtual-machine-SQLserver) 允许你在云中使用完整版本的 SQL Server，不需管理任何本地硬件。 使用即用即付时，SQL Server VM 还可以简化许可成本。

Azure 虚拟机在中国的 2 个不同[地理区域](https://www.azure.cn/support/service-dashboard/)运行， 并提供各种[虚拟机大小](../sizes.md)。 使用虚拟机映像库可以创建 SQL Server VM，而且版本和操作系统都很正确。 因此，虚拟机适用于许多不同的 SQL Server 工作负荷。

## <a name="automated-updates"></a>自动更新

SQL Server Azure VM 可以使用[自动修补](virtual-machines-windows-sql-automated-patching.md)来安排维护时段，以便自动安装重要的 Windows 和 SQL Server 更新。

## <a name="automated-backups"></a>自动备份

SQL Server Azure VM 可以利用[自动备份](virtual-machines-windows-sql-automated-backup-v2.md)，定期创建数据库到 Blob 存储的备份。 也可以手动使用此技术。 有关详细信息，请参阅[使用 Azure 存储进行 SQL Server 备份和还原](virtual-machines-windows-use-storage-sql-server-backup-restore.md)。

## <a name="high-availability"></a>高可用性

如果需要高可用性，请考虑配置 SQL Server 可用性组。 这涉及虚拟网络中的多个 SQL Server Azure VM。 可以手动配置高可用性解决方案，也可以在 Azure 门户中使用模板进行自动配置。 有关所有高可用性选项的概述，请参阅 [Azure 虚拟机中 SQL Server 的高可用性和灾难恢复](virtual-machines-windows-sql-high-availability-dr.md)。

## <a name="performance"></a>性能

Azure 虚拟机提供的虚拟机大小取决于工作负荷需求。 SQL VM 还提供按性能要求优化的自动存储配置。 若要详细了解如何为 SQL VM 配置存储，请参阅 [SQL Server VM 的存储配置](virtual-machines-windows-sql-server-storage-configuration.md)。 若要优化性能，请参阅 [Azure 虚拟机中 SQL Server 的性能最佳做法](virtual-machines-windows-sql-performance.md)。

## <a name="get-started-with-sql-vms"></a>SQL VM 入门

若要开始，请选择一个 SQL Server 虚拟机映像，其中包含所需的版本和操作系统。 下面的各部分针对相关 SQL Server 虚拟机库映像提供了指向 Azure 门户的直接链接。

> [!TIP]
> 若要了解这些映像的 VM 和 SQL 定价，请参阅 [SQL Server Azure VM 的定价指南](virtual-machines-windows-sql-server-pricing-guidance.md)。

> [!TIP]
> 若要了解 SQL Server 虚拟机库映像的更新和生命周期策略，请参阅 [SQL Server VM 常见问题解答](virtual-machines-windows-sql-server-iaas-faq.md#images)。

### <a id="payasyougo"></a> 即用即付
下表提供了一个矩阵，其中包含即用即付 SQL Server 映像。

| 版本 | 操作系统 | 版本 |
| --- | --- | --- |
| **SQL Server 2017** |Windows Server 2016 |[Enterprise](https://portal.azure.cn/#create/Microsoft.SQLServer2017EnterpriseWindowsServer2016)、[Standard](https://portal.azure.cn/#create/Microsoft.SQLServer2017StandardonWindowsServer2016)、[Web](https://portal.azure.cn/#create/Microsoft.SQLServer2017WebonWindowsServer2016)、[Express](https://portal.azure.cn/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonWindowsServer2016)、[Developer](https://portal.azure.cn/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonWindowsServer2016) |
| **SQL Server 2016 SP1** |Windows Server 2016 |[Enterprise](https://portal.azure.cn/#create/Microsoft.SQLServer2016SP1EnterpriseWindowsServer2016)、[Standard](https://portal.azure.cn/#create/Microsoft.SQLServer2016SP1StandardWindowsServer2016)、[Web](https://portal.azure.cn/#create/Microsoft.SQLServer2016SP1WebWindowsServer2016)、[Express](https://portal.azure.cn/#create/Microsoft.SQLServer2016SP1ExpressWindowsServer2016)、[Developer](https://portal.azure.cn/#create/Microsoft.SQLServer2016SP1DeveloperWindowsServer2016) |
| **SQL Server 2014 SP2** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.cn/#create/Microsoft.SQLServer2014SP2EnterpriseWindowsServer2012R2)、[Standard](https://portal.azure.cn/#create/Microsoft.SQLServer2014SP2StandardWindowsServer2012R2)、[Web](https://portal.azure.cn/#create/Microsoft.SQLServer2014SP2WebWindowsServer2012R2)、[Express](https://portal.azure.cn/#create/Microsoft.SQLServer2014SP2ExpressWindowsServer2012R2) |
| **SQL Server 2012 SP3** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.cn/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2)、[Standard](https://portal.azure.cn/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2)、[Web](https://portal.azure.cn/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2)、[Express](https://portal.azure.cn/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2) |

<!-- Not Available on  [Overview of SQL Server on Azure Virtual Machines (Linux)](../../linux/sql/sql-server-linux-virtual-machines-overview.md) -->
<!-- Not Available on ### <a id="BYOL"></a> Bring your own license-->

### <a name="connect-to-the-vm"></a>连接到 VM
创建 SQL Server VM 以后，即可从 SQL Server Management Studio (SSMS) 之类的应用程序或工具连接到该 VM。 有关说明，请参阅[连接到 Azure 上的 SQL Server 虚拟机](virtual-machines-windows-sql-connect.md)。

### <a name="migrate-your-data"></a>迁移数据
如果已有数据库，会想要将该数据库移至新预配的 SQL VM。 有关迁移选项的列表和指导，请参阅[将数据库迁移到 Azure VM 上的 SQL Server](virtual-machines-windows-migrate-sql.md)。

## <a name="customer-experience-improvement-program-ceip"></a>客户体验改善计划 (CEIP)
客户体验改善计划 (CEIP) 默认情况下已启用。 这样会定期将报告发送至 Microsoft，帮助改进 SQL Server。 CEIP 不要求管理任务，除非想在预配后禁用它。 可以通过远程桌面连接到 VM，以自定义或禁用 CEIP。 然后运行 **SQL Server 错误和使用情况报告**实用工具。 请按照说明禁用报告功能。 有关数据收集的详细信息，请参阅 [SQL Server 隐私声明](https://www.microsoft.com/EN-US/privacystatement/SQLServer/Default.aspx)。

## <a name="related-products-and-services"></a>相关产品和服务
### <a name="windows-virtual-machines"></a>Windows 虚拟机
* [虚拟机概述](../overview.md)

### <a name="storage"></a>存储
* [Azure 存储简介](../../../storage/common/storage-introduction.md)

### <a name="networking"></a>网络
* [虚拟网络概述](../../../virtual-network/virtual-networks-overview.md)
* [Azure 中的 IP 地址](../../../virtual-network/virtual-network-ip-addresses-overview-arm.md)
* [在 Azure 门户中创建完全限定的域名](../portal-create-fqdn.md)

### <a name="sql"></a>SQL
* [SQL Server 文档](https://docs.microsoft.com/sql/index)
* [Azure SQL 数据库比较](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md)

## <a name="next-steps"></a>后续步骤

Azure 虚拟机上的 SQL Server 入门：

* [在 Azure 门户中创建 SQL Server VM](quickstart-sql-vm-create-portal.md)

获取有关 SQL VM 的常见问题的解答：

* [Azure 虚拟机中的 SQL Server 常见问题](virtual-machines-windows-sql-server-iaas-faq.md)
<!--Update_Description: update meta properties, wording update -->