---
title: "有效管理 Azure 虚拟机上 SQL Server 的成本 | Azure"
description: "提供选择适当 SQL Server 虚拟机定价模型的最佳做法。"
services: virtual-machines-windows
documentationcenter: na
author: luisherring
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
origin.date: 04/18/2017
ms.date: 07/03/2017
ms.author: v-dazen
ms.openlocfilehash: fecb1d9b3c3ebbd8584cd995439ad5875807e550
ms.sourcegitcommit: 2c397ac599bdb39b257580a1b55a1ce67e19ae56
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="pricing-guidance-for-sql-server-azure-vms"></a>SQL Server Azure VM 的定价指南

本主题提供 Azure 中的 SQL Server 虚拟机的定价指南。 有几个选项会影响成本，请务必选取适当的映像来平衡成本与业务需求。

## <a name="free-licensed-sql-server-editions"></a>SQL Server 免费授权版

若要开发、测试或生成概念证明，请使用免费授权的“SQL Server Developer 版本”。 此版本具有 SQL Server Enterprise 版本中的所有内容，因此可用于构建所需的任意应用程序。 该版本只是不可在生产环境中运行。 SQL Server Developer VM 将仅收取 VM 的费用，而不收取 SQL Server 许可的费用。

若要在生产环境中运行轻型工作负荷（小于 4 核、小于 1 GB 内存、小于 10 GB/数据库），请使用免费授权的“SQL Server Express 版本”。 SQL Express VM 将仅收取 VM 的费用，而不收取 SQL 许可的费用。

对于这些开发/测试或轻型生产工作负荷，还可通过选择与这些工作负荷相匹配的较小的 VM 大小来节省资金。 DS1v2 可能是这些工作负荷的不错选择。

若要使用上述某个映像创建 SQL Server 2016 Azure VM，请参阅以下链接：

- [SQL Server 2016 Developer Azure VM](https://portal.azure.cn/#create/Microsoft.FreeLicenseSQLServer2016SP1DeveloperWindowsServer2016-ARM)
- [SQL Server 2016 Express Azure VM](https://portal.azure.cn/#create/Microsoft.FreeLicenseSQLServer2016SP1ExpressWindowsServer2016-ARM)

## <a name="paid-sql-server-editions"></a>SQL Server 付费版

若拥有非轻型生产工作负荷，请使用以下 SQL Server 版本之一：

| SQL Server 版本 | 工作负载 |
|-----|-----|
| Web | 小型网站 |
| 标准 | 中小型工作负荷 |
| Enterprise | 大型或任务关键型工作负荷|

可按两种方法为这些版本的 SQL Server 许可付费：按使用情况付费或自带许可证。

### <a name="pay-per-usage"></a>按使用情况付费

“按使用情况支付 SQL Server 许可证费用”意味着 Azure VM 的每分钟运行成本包括 SQL Server 许可证的费用。 有关不同 SQL Server 版本（Web、Standard 和 Enterprise）的定价，可参阅 [Azure VM 定价页](https://www.azure.cn/pricing/details/virtual-machines/)。 所有版本的 SQL Server（2008 R2 到 2016）的费用均相同。 通常与 SQL Server 许可一样，每分钟许可费用取决于 VM 内核数。

建议在以下情况采用“按使用情况支付 SQL Server 许可费用”：

- 临时或定期工作负荷。 例如，某应用每年需支持某事件几个月，或需在星期一支持业务分析。
- 生存期或规模未知的工作负荷。 例如，某应用可能在几个月内无需使用，或可能需要提高/降低计算能力（具体取决于需求）。

若要使用上述某个按使用情况付费的映像创建 SQL Server 2016 Azure VM，请参阅以下链接：

- [SQL Server 2016 Web Azure VM](https://portal.azure.cn/#create/Microsoft.SQLServer2016SP1WebWindowsServer2016)
- [SQL Server 2016 Standard Azure VM](https://portal.azure.cn/#create/Microsoft.SQLServer2016SP1StandardWindowsServer2016)
- [SQL Server 2016 Enterprise Azure VM](https://portal.azure.cn/#create/Microsoft.SQLServer2016SP1EnterpriseWindowsServer2016)

> [!IMPORTANT]
> 在 Azure 门户中创建 SQL Server 虚拟机时，“选择大小”边栏选项卡上显示的每月预估成本不包括 SQL Server 许可成本。 这只是 VM 的成本。
>
> ![“选择 VM 大小”边栏选项卡](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-choose-size-pricing-estimate.png)
>
>对于 SQL Server 的 Express 和 Developer 免费授权版，这是指总预估成本。 但对于 Web、Standard 和 Enterprise 版本，请在 [Windows 虚拟机定价页](https://www.azure.cn/pricing/details/virtual-machines/windows/)上查看额外的 SQL 许可成本。 在定价页上，选择 SQL Server 的目标版本。

## <a name="avoid-unecessary-costs"></a>避免不必要的成本

若要使用任何不连续运行的工作负荷，请考虑在非活动期间关闭虚拟机。 仅为所用的部分付费。

例如，如果只在 Azure VM 上试用 SQL Server，就不会希望因使其意外运行数周而滋生费用。 一种解决方案是使用[自动关闭功能](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/)。

![SQL VM 自动关闭](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-auto-shutdown.png)

对于其他工作流，请考虑使用脚本解决方案（如 [Azure 自动化](https://www.azure.cn/home/features/automation/)）自动关闭并重启 Azure VM。

> [!IMPORTANT]
> 关闭和取消分配 VM 是避免产生费用的唯一方法。 只停止或使用电源选项关闭 VM 仍会产生使用费。

## <a name="next-steps"></a>后续步骤

有关虚拟机最新定价（包括 SQL Server），请参阅 [Azure VM 定价页](https://www.azure.cn/pricing/details/virtual-machines/)。

查看 [Azure 虚拟机上的 SQL Server 概述](virtual-machines-windows-sql-server-iaas-overview.md)中的其他 SQL Server 虚拟机主题。