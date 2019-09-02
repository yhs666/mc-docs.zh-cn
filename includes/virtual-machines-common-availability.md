---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 05/10/2019
ms.date: 08/12/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: bae70ffbf47083f2b5ed2692532393509c9ac52f
ms.sourcegitcommit: 8ac3d22ed9be821c51ee26e786894bf5a8736bfc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2019
ms.locfileid: "68913060"
---
# <a name="availability-options-for-virtual-machines-in-azure"></a>Azure 中虚拟机的可用性选项
本文概述了 Azure 虚拟机 (VM) 的可用性功能。

## <a name="availability-sets"></a>可用性集
可用性集是数据中心内的 VM 的逻辑分组，可让 Azure 了解应用程序的构建方式，以便提供冗余和可用性。 建议在可用性集内创建两个或多个 VM，提供高度可用的应用程序，并满足 [99.95% Azure SLA](https://www.azure.cn/support/sla/virtual-machines/) 的要求。 可用性集本身是免费的，只需为创建的每个 VM 实例付费。 当单个 VM 使用 [Azure 高级 SSD](../articles/virtual-machines/windows/disks-types.md#premium-ssd) 时，Azure SLA 适用于计划外维护事件。

可用性集由可防止硬件故障以及允许安全应用更新的两个额外分组构成 - 容错域 (FD) 和更新域 (UD)。 详细了解如何管理 [Linux VM](../articles/virtual-machines/linux/manage-availability.md) 或 [Windows VM](../articles/virtual-machines/windows/manage-availability.md) 的可用性。

### <a name="fault-domains"></a>容错域
容错域是共享公用电源和网络交换机的基础硬件逻辑组，类似于本地数据中心内的机架。 在可用性集内创建 VM 时，Azure 平台会将 VM 自动分布到这些容错域。 此方法可限制潜在物理硬件故障、网络中断或断电的影响。

### <a name="update-domains"></a>更新域
更新域是可以同时维护或重新启动的基础硬件逻辑组。 在可用性集内创建 VM 时，Azure 平台会自动将 VM 分布到这些更新域。 Azure 平台进行定期维护时，此方法可确保至少有一个应用程序实例始终保持运行状态。 在计划内维护期间，更新域的重启顺序可能不会按序进行，但一次只重启一个更新域。

![可用性集](./media/virtual-machines-common-manage-availability/ud-fd-configuration.png)

### <a name="managed-disk-fault-domains"></a>托管磁盘容错域
对于使用 [Azure 托管磁盘](../articles/virtual-machines/windows/faq-for-disks.md)的 VM，在使用托管可用性集时，VM 与托管磁盘容错域一致。 该一致性可确保附加到 VM 的所有托管磁盘都在同一托管磁盘容错域内。 在托管可用性集中，只能创建带托管磁盘的 VM。 托管磁盘容错域的数目因区域而异 - 每个区域两个或三个托管磁盘容错域。 可以阅读有关这些适用于 [Linux VM](../articles/virtual-machines/linux/manage-availability.md?#use-managed-disks-for-vms-in-an-availability-set) 或 [Windows VM](../articles/virtual-machines/windows/manage-availability.md?#use-managed-disks-for-vms-in-an-availability-set) 的托管磁盘容错域的详细信息。

![托管可用性集](./media/virtual-machines-common-manage-availability/md-fd-updated.png)

<!--Not Available on ## Availability zones-->


## <a name="next-steps"></a>后续步骤
现在即可开始使用这些可用性和冗余功能构建 Azure 环境。 有关最佳做法的信息，请参阅 [Azure 可用性最佳做法](../articles/best-practices-availability-checklist.md)。

<!--Update_Description: new articles on virtual machines common availability -->
<!--ms.date: 08/12/2019-->