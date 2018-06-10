---
title: Azure 存储中的数据复制 | Microsoft Docs
description: 复制 Azure 存储帐户中的数据，实现持久性和高可用性。 复制选项包括本地冗余存储 (LRS)、区域冗余存储 (ZRS)、异地冗余存储 (GRS) 和读取访问异地冗余存储 (RA-GRS)。
services: storage
author: forester123
manager: josefree
ms.service: storage
ms.topic: article
origin.date: 01/21/2018
ms.date: 06/11/2018
ms.author: v-nany
ms.openlocfilehash: 10b5ee59faad10ac5dad634b300211e4f0385cb1
ms.sourcegitcommit: 49c8c21115f8c36cb175321f909a40772469c47f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2018
ms.locfileid: "34867521"
---
# <a name="azure-storage-replication"></a>Azure 存储复制

始终复制 Azure 存储帐户中的数据，确保持久性和高可用性。 Azure 存储复制功能可以复制数据，以防范各种计划内和计划外的事件，例如暂时性的硬件故障、网络中断或断电、大范围自然灾害等。 可以选择在同一数据中心以及甚至跨区域复制数据。

即使面临故障时，复制也可确保存储帐户满足[存储的服务级别协议 (SLA)](https://www.azure.cn/zh-cn/support/sla/storage/)的要求。 请参阅 SLA，了解有关 Azure 存储确保持续性和可用性的信息。

## <a name="choosing-a-replication-option"></a>选择复制选项

创建存储帐户时，可以选择以下复制选项之一：

* [本地冗余存储 (LRS)](storage-redundancy-lrs.md)
* [异地冗余存储 (GRS)](storage-redundancy-grs.md)
* [读取访问异地冗余存储 (RA-GRS)](storage-redundancy-grs.md#read-access-geo-redundant-storage)

| 方案 | LRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |
| 数据中心内的节点不可用 |是 |是 |是
| 整个数据中心（区域性或非区域性）不可用 |否 |是 |是 |
| 区域范围的服务中断 |否 |是 |是 |
| 整个区域不可用时对数据进行读取访问（远程异地复制区域中） |否 |否 |是 |
| 旨在给定年份为对象提供 ___ 的持续性 |至少 99.999999999%（11 个 9）|至少 99.99999999999999%（16 个 9）|至少 99.99999999999999%（16 个 9）|
| 提供 ___ 种存储帐户类型 |GPv1，Blob |GPv1，Blob |GPv1，Blob

有关不同冗余选项的定价信息，请参阅 [Azure 存储定价](https://www.azure.cn/pricing/details/storage/)。

> [!NOTE]
> 高级存储仅支持本地冗余存储 (LRS)。 有关高级存储的信息，请参阅[高级存储：适用于 Azure 虚拟机工作负荷的高性能存储](../../virtual-machines/windows/premium-storage.md)。

## <a name="changing-replication-strategy"></a>更改复制策略
我们允许使用 [Azure 门户](https://portal.azure.cn/)、[Azure Powershell](storage-powershell-guide-full.md)、[Azure CLI](/cli/install-azure-cli?view=azure-cli-latest) 或众多的 [Azure 客户端库](https://docs.azure.cn/index?view=azure-dotnet#pivot=sdkstools)之一来更改存储帐户的复制策略。 更改存储帐户的复制类型不会导致停机。

### <a name="are-there-any-costs-to-changing-my-accounts-replication-strategy"></a>更改帐户的复制策略是否产生任何费用？
这取决于转换路径。 从最便宜到最昂贵的冗余产品/服务依次为 LRS、GRS 和 RA-GRS。 例如，从 LRS 转移到其他任何存储会产生额外的费用，因为这是转移到更高级的冗余级别。 转移到 GRS 或 RA-GRS 会产生出口带宽费用，因为（主要区域中的）数据将复制到远程次要区域。 这是在初始设置期间收取的一次性费用。 复制数据后，无需进一步支付转换费用。 只有在复制任何新数据，或者复制现有数据的更新时才要付费。 有关带宽费用的详细信息，请参阅 [Azure 存储定价页面](https://www.azure.cn/pricing/details/storage/blobs/)。

如果从 GRS 更改为 LRS，则不会产生额外的费用，但从次要位置复制的数据将被删除。

## <a name="see-also"></a>另请参阅

- [本地冗余存储 (LRS)：Azure 存储的低成本数据冗余](storage-redundancy-lrs.md)
- [异地冗余存储 (GRS)：Azure 存储的跨区域复制](storage-redundancy-grs.md)
- [Azure 存储可伸缩性和性能目标](storage-scalability-targets.md)
- [使用 RA-GRS 存储设计高度可用的应用程序](../storage-designing-ha-apps-with-ragrs.md)
- [Azure 存储冗余选项和读取访问异地冗余存储](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)
- [SOSP Paper - Azure Storage: A highly available cloud storage service with strong consistency（SOSP 论文 - Azure 存储：具有高度一致性的高可用云存储服务）](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)
<!--Update_Description: main content struture update-->