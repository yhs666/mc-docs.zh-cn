---
title: "有效管理 Azure 虚拟机上 SQL Server 的成本 | Azure"
description: "提供选择适当 SQL Server 虚拟机定价模型的最佳做法。"
services: virtual-machines-windows
documentationcenter: na
author: rockboyfor
manager: digimobile
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
origin.date: 10/17/2017
ms.date: 01/08/2018
ms.author: v-yeche
ms.openlocfilehash: d331e75639b2f2c32aa07509b57bd40157295406
ms.sourcegitcommit: f02cdaff1517278edd9f26f69f510b2920fc6206
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/05/2018
---
# <a name="pricing-guidance-for-sql-server-azure-vms"></a>SQL Server Azure VM 的定价指南

本文提供 Azure 的 SQL Server 虚拟机的定价指南。 有几个选项会影响成本，请务必选取适当的映像来平衡成本与业务需求。

## <a name="free-licensed-sql-server-editions"></a>SQL Server 免费授权版

若要开发、测试或生成概念证明，请使用免费授权的“SQL Server Developer 版本”。 此版本具有 SQL Server Enterprise 版本中的所有内容，因此可用于构建所需的任意应用程序。 此版本只是不可在生产环境中运行。 A SQL Server Developer VM 仅收取 VM 的费用，而不收取 SQL Server 许可的费用。

若要在生产环境中运行轻型工作负荷（小于 4 核、小于 1 GB 内存、小于 10 GB/数据库），请使用免费授权的“SQL Server Express 版本”。 SQL Express VM 仅收取 VM 的费用，而不收取 SQL 许可的费用。

对于这些开发/测试或轻型生产工作负荷，还可通过选择与这些工作负荷相匹配的较小的 VM 大小来节省资金。 DS1v2 可能是这些工作负荷的不错选择。

若要使用上述某个映像创建 SQL Server 2017 Azure VM，请参阅以下链接：

| 平台 | 自由许可的映像 |
|---|---|
| Windows Server 2016 | [SQL Server 2017 Developer Azure VM](https://portal.azure.cn/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonWindowsServer2016)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.cn/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonWindowsServer2016) |
<!--Not Available on Red Hat Enterprise Linux  -->
<!--Not Available on SUSE Linux Enterprise Server  -->
<!--Not Available on Red Hat Enterprise Linux  -->

## <a name="paid-sql-server-editions"></a>SQL Server 付费版

若拥有非轻型生产工作负荷，请使用以下 SQL Server 版本之一：

| SQL Server 版本 | 工作负载 |
|-----|-----|
| Web | 小型网站 |
| Standard | 中小型工作负荷 |
| Enterprise | 大型或任务关键型工作负荷|

可按一种方法为这些版本的 SQL Server 许可付费：按使用情况付费。

### <a name="pay-per-usage"></a>按使用情况付费

“按使用情况支付 SQL Server 许可证费用”意味着 Azure VM 的每分钟运行成本包括 SQL Server 许可证的费用。 有关不同 SQL Server 版本（Web、Standard 和 Enterprise）的定价，可参阅 [Azure VM 定价页](https://www.azure.cn/pricing/details/virtual-machines/)。 所有版本的 SQL Server（2012 SP3 到 2017）的费用相同。 通常与 SQL Server 许可一样，每分钟许可费用取决于 VM 内核数。

建议在以下情况采用“按使用情况支付 SQL Server 许可费用”：

- 临时或定期工作负荷。 例如，某应用每年需支持某事件几个月，或需在星期一支持业务分析。
- 生存期或规模未知的工作负荷。 例如，某应用可能在几个月内无需使用，或可能需要提高/降低计算能力（具体取决于需求）。

若要使用上述某个按使用情况付费的映像创建 SQL Server 2017 Azure VM，请参阅以下链接：

| 平台 | 许可的映像 |
|---|---|
| Windows Server 2016 | [SQL Server 2017 Web Azure VM](https://portal.azure.cn/#create/Microsoft.SQLServer2017WebonWindowsServer2016)<br/>[SQL Server 2017 Standard Azure VM](https://portal.azure.cn/#create/Microsoft.SQLServer2017StandardonWindowsServer2016)<br/>[SQL Server 2017 Enterprise Azure VM](https://portal.azure.cn/#create/Microsoft.SQLServer2017EnterpriseWindowsServer2016) |
<!--Not Available on Red Hat Enterprise Linux  -->
<!--Not Available on SUSE Linux Enterprise Server  -->
<!--Not Available on Red Hat Enterprise Linux  -->

> [!IMPORTANT]
> 在 Azure 门户中创建 SQL Server 虚拟机时，“选择大小”窗口显示估算成本。 请务必注意，此估算成本仅仅是运行 VM 所产生的计算成本，以及 Windows VM 的所有 Windows 许可成本， 而不包括 Web、Standard 和 Enterprise 版本的其他 SQL Server 许可成本。 若要获取最准确的估算定价，请在定价页上选择 [Windows](https://www.azure.cn/pricing/details/virtual-machines/) 的操作系统与 SQL Server 版本。
>
> ![“选择 VM 大小”边栏选项卡](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-choose-size-pricing-estimate.png)

<!-- Not Available on ### Bring your own license (BYOL) -->


## <a name="avoid-unnecessary-costs"></a>避免不必要的成本

为了避免不必要的成本，请选择最佳的虚拟机大小，并考虑间歇性关闭非连续的工作负荷。

### <a id="machinesize"></a>正确调整 VM 的大小

SQL Server 的许可成本直接与核心数相关。 请选择与 CPU、内存、存储和 I/O 带宽需求相符的 VM 大小。 有关计算机大小选项的完整列表，请参阅 [Windows VM 大小](/virtual-machines/windows/sizes)和 [Linux VM 大小](/virtual-machines/linux/sizes?toc=%2fvirtual-machines%2flinux%2ftoc.json)。

有一些新的计算机大小能够很好地适应特定类型的 SQL Server 工作负荷。 这些计算机大小保留较高级别的内存、存储和 I/O 带宽，但虚拟化核心数较少。 例如，考虑以下情况：

| VM 大小 | vCPU | 内存 | 最大磁盘数 | 最大 I/O 吞吐量 | SQL 许可成本 | 总成本（计算 + 许可） |
|---|---|---|---|---|---|---|
| **Standard_DS14v2** | 16 | 112 GB | 32 | 51,200 IOPS 或 768 MB/秒 | | |
| **Standard_DS14-4v2** | 4 | 112 GB | 32 | 51,200 IOPS 或 768 MB/秒 | 低 75% | 低 57% |

> [!IMPORTANT]
> 这是一个时间点示例。 有关最新的规范，请参阅适用于 [Windows](https://www.azure.cn/pricing/details/virtual-machines/) 和 [Linux](https://www.azure.cn/pricing/details/virtual-machines/) 的计算机大小文章和 Azure 定价页。

在前面的示例中可以看到，除 vCPU 以外，**Standard_DS14v2** 和 **Standard_DS14-4v2** 的规范在其他方面相同。 **Standard_DS14-4v2** 计算机大小末尾的后缀 **-4v2** 表示活动的 vCPU 数目。 由于 SQL Server 许可成本与核心数密切相关，因此，在不需要额外的 vCPU 的情况下，此计算机大小可以大幅降低 VM 的成本。 这只是其中一个示例，还有许多计算机大小的 vCPU 数目受到限制，具体数字以此后缀模式标识。 有关详细信息，请参阅博客文章 [Announcing new Azure VM sizes for more cost-effective database work](https://azure.microsoft.com/blog/announcing-new-azure-vm-sizes-for-more-cost-effective-database-workloads/)（宣布推出新的 Azure VM 大小来提高数据库工作的性价比）。

### <a name="shut-down-your-vm-when-possible"></a>在可能的情况下关闭 VM

若要使用任何不连续运行的工作负荷，请考虑在非活动期间关闭虚拟机。 仅为所用的部分付费。

例如，如果只在 Azure VM 上试用 SQL Server，就不会希望因使其意外运行数周而滋生费用。 一种解决方案是使用[自动关闭功能](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/)。

![SQL VM 自动关闭](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-auto-shutdown.png)
<!-- Not Available on  [Azure DevTest Labs](https://www.azure.cn/home/features/devtest-lab/). -->

对于其他工作流，请考虑使用脚本解决方案（如 [Azure 自动化](https://www.azure.cn/home/features/automation/)）自动关闭并重启 Azure VM。

> [!IMPORTANT]
> 关闭和取消分配 VM 是避免产生费用的唯一方法。 只停止或使用电源选项关闭 VM 仍会产生使用费。

## <a name="next-steps"></a>后续步骤

<!-- Not Available on [Prevent unexpected costs with Azure billing and cost management](../../../billing/billing-getting-started.md).  -->
有关虚拟机最新定价（包括 SQL Server），请参阅 [Azure VM 定价页](https://www.azure.cn/pricing/details/virtual-machines/)。

在 [SQL Server Windows VM](virtual-machines-windows-sql-server-iaas-overview.md) 中详细了解 SQL Server 虚拟机。
<!-- Not Available on  and [SQL Server Linux VMs](../../linux/sql/sql-server-linux-virtual-machines-overview.md) -->
<!-- Update_Description: update meta properties, wording update, update link -->