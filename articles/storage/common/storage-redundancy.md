---
title: "Azure 存储中的数据复制 | Microsoft Docs"
description: "复制 Azure 存储帐户中的数据，实现持久性和高可用性。 复制选项包括本地冗余存储 (LRS)、区域冗余存储 (ZRS)、异地冗余存储 (GRS) 和读取访问异地冗余存储 (RA-GRS)。"
services: storage
documentationcenter: 
author: forester123
manager: digimobile
editor: tysonn
ms.assetid: 86bdb6d4-da59-4337-8375-2527b6bdf73f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 05/15/2017
ms.date: 10/30/2017
ms.author: v-johch
ms.openlocfilehash: 788505312680578089363e177ceab31adc26eb03
ms.sourcegitcommit: f02cdaff1517278edd9f26f69f510b2920fc6206
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/05/2018
---
# <a name="azure-storage-replication"></a>Azure 存储复制

始终复制 Azure 存储帐户中的数据，确保持久性和高可用性。 根据所选的复制选项，复制操作将在同一数据中心内复制数据或将其复制到另一个数据中心。 复制可保护数据，并在发生暂时性硬件故障时保留应用程序正常运行时间。 如果数据复制到第二个数据中心，还可以保护数据，以免主要位置发生灾难性故障。

即使面临故障时，复制也可确保存储帐户满足[存储的服务级别协议 (SLA)](https://www.azure.cn/support/sla/storage/)的要求。 请参阅 SLA，了解有关 Azure 存储确保持续性和可用性的信息。

创建存储帐户时，可以选择以下复制选项之一：

* [本地冗余存储 (LRS)](#locally-redundant-storage)
* [区域冗余存储空间 (ZRS)](#zone-redundant-storage)
* [异地冗余存储 (GRS)](#geo-redundant-storage)
* [读取访问异地冗余存储 (RA-GRS)](#read-access-geo-redundant-storage)

创建存储帐户时，默认选项为读取访问异地冗余存储 (RA-GRS)。

下表简要概述了 LRS、ZRS、GRS 和 RA-GRS 之间的差异，而后续章节将详细介绍每种类型的复制。

| 复制策略 | LRS | ZRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |:--- |
| 数据在多个数据中心之间进行复制。 |否 |是 |是 |是 |
| 可以从辅助位置和主位置读取数据。 |否 |否 |否 |是 |
| 设计为在给定的一年内提供 ___ 对象持久性。 |至少 99.999999999%（11 个 9）|至少 99.9999999999%（12 个 9）|至少 99.99999999999999%（16 个 9）|至少 99.99999999999999%（16 个 9）|

有关不同冗余选项的定价信息，请参阅 [Azure 存储定价](https://www.azure.cn/pricing/details/storage/)。

> [!NOTE]
> 高级存储仅支持本地冗余存储 (LRS)。 有关高级存储的信息，请参阅[高级存储：适用于 Azure 虚拟机工作负荷的高性能存储](../storage-premium-storage.md)。
>

## <a name="locally-redundant-storage"></a>本地冗余存储
[!INCLUDE [storage-common-redundancy-LRS](../../../includes/storage-common-redundancy-LRS.md)]

## <a name="zone-redundant-storage"></a>区域冗余存储
除了像 LRS 一样存储三个副本，区域冗余存储空间 (ZRS) 还会在一两个区域的数据中心之间异步复制数据，提供高于 LRS 的持久性。 即使主数据中心不可用或不可恢复，存储在 ZRS 中的数据也是持久的。
计划使用 ZRS 的客户需注意：

* ZRS 仅可用于通用存储帐户中的块 blob，并且仅在存储服务版本 2014-02-14 及更高版本中受支持。
* 由于异步复制涉及延迟，因此在遇到本地灾难时，如果无法从主要区域中恢复数据，则可能会丢失尚未复制到次要区域的更改。
* 在 Microsoft 启动故障转移到次要区域之前，该副本可能不可用。
* 随后，ZRS 帐户无法转换为 LRS 或 GRS。 同样，现有 LRS 或 GRS 帐户无法转换为 ZRS 帐户。
* ZRS 帐户不具有度量值或日志记录功能。

## <a name="geo-redundant-storage"></a>异地冗余存储
[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-GRS.md)]

## <a name="read-access-geo-redundant-storage"></a>读取访问异地冗余存储
除了在 GRS 所提供的两个区域之间进行复制外，读取访问异地冗余存储 (RA-GRS) 还提供对次要位置中的数据的只读访问权限，从而在最大程度上提高存储帐户的可用性。

当启用对次要区域中的数据的只读访问权限时，除了存储帐户的主终结点外，还可以在辅助终结点上获取数据。 辅助终结点与主终结点类似，但会在帐户名称后面追加后缀 `-secondary` 。 例如，如果 Blob 服务的主终结点是 `myaccount.blob.core.chinacloudapi.cn`，则辅助终结点是 `myaccount-secondary.blob.core.chinacloudapi.cn`。 存储帐户的访问密钥对于主终结点和辅助终结点是相同的。

注意事项：

* 应用程序必须管理在使用 RA-GRS 时要与哪些终结点进行交互。
* 由于异步复制涉及延迟，因此在遇到区域性灾难时，如果无法从主要区域中恢复数据，则可能会丢失尚未复制到次要区域的更改。
* 如果 Azure 启动了到次要区域的故障转移，则在故障转移完成以后，用户将对该数据具有读取和写入访问权限。 有关详细信息，请参阅[灾难恢复指南](../storage-disaster-recovery-guidance.md)。 
* 要实现高可用性时使用 RA-GRS。 有关可伸缩性的指南，请查看[性能清单](../storage-performance-checklist.md)。

## <a name="frequently-asked-questions"></a>常见问题

<a id="howtochange"></a>
#### <a name="1-how-can-i-change-the-geo-replication-type-of-my-storage-account"></a>1.如何更改我的存储帐户的异地复制类型？

   可以使用 [Azure 门户](https://portal.azure.cn/)、[Azure Powershell](storage-powershell-guide-full.md) 或以编程方式使用众多存储客户端库之一将存储帐户的异地复制类型在 LRS、GRS 和 RA-GRS 之间进行更改。 请注意，ZRS 帐户无法转换为 LRS 或 GRS。 同样，现有 LRS 或 GRS 帐户无法转换为 ZRS 帐户。

<a id="changedowntime"></a>
#### <a name="2-will-there-be-any-down-time-if-i-change-the-replication-type-of-my-storage-account"></a>2.更改我的存储帐户的复制类型时是否会有任何停机时间？

   不会，不会有任何停机时间。

<a id="changecost"></a>
#### <a name="3-will-there-be-any-additional-cost-if-i-change-the-replication-type-of-my-storage-account"></a>3.更改我的存储帐户的复制类型是否会有任何额外成本？

   是的。 如果将你的存储帐户从 LRS 更改为 GRS（或 RA-GRS），对于将现有数据从主位置复制到辅助位置时涉及的出口，将产生额外的费用。 在复制初始数据后，对于从主位置到辅助位置的数据异地复制，不再产生额外的出口费用。 有关带宽费用的详细信息，请参阅 [Azure 存储定价页](https://www.azure.cn/pricing/details/storage)。 如果从 GRS 更改到 LRS，不会有额外的成本，但会将你的数据从辅助位置中删除。

<a id="ragrsbenefits"></a>
#### <a name="4-how-can-ra-grs-help-me"></a>4.RA-GRS 对我有何帮助？

   GRS 存储将数据从主要区域复制到距主要区域数百英里的次要区域。 在这种情况下，即使在遇到区域完全停电或导致主要区域不可恢复的灾难时，数据也能持久保存。 RA-GRS 存储包括此附加功能，它增加了从辅助位置读取数据的能力。 有关如何利用此能力的一些观点，请参阅[设计使用 RA-GRS 存储的具有高可用性的应用程序](../storage-designing-ha-apps-with-ragrs.md)。 

<a id="lastsynctime"></a>
#### <a name="5-is-there-a-way-for-me-to-figure-out-how-long-it-takes-to-replicate-my-data-from-the-primary-to-the-secondary-region"></a>5.是否可以计算将我的数据从主要区域复制到次要区域需要花费多长时间？

   如果使用的是 RA-GRS 存储，可以查看你的存储帐户的上次同步时间。 上次同步时间是一个 GMT 日期/时间值；在上次同步时间之前发生的所有主位置写入都已成功写入到辅助位置，这意味着可以从辅助位置读取它们。 在上次同步时间之后发生的主位置写入可能可供读取，也可能尚不可供读取。 可以使用 [Azure 门户](https://portal.azure.cn/)、[Azure PowerShell](storage-powershell-guide-full.md) 或以编程方式使用 REST API 或存储客户端库之一查询此值。 

<a id="outage"></a>
#### <a name="6-how-can-i-switch-to-the-secondary-region-if-there-is-an-outage-in-the-primary-region"></a>6.如果主要区域中发生中断，如何切换到次要区域？

   有关更多详细信息，请参阅[发生 Azure 存储中断时该怎么办](../storage-disaster-recovery-guidance.md)。

<a id="rpo-rto"></a>
#### <a name="7-what-is-the-rpo-and-rto-with-grs"></a>7.什么是采用 GRS 的 RPO 和 RTO？
   
   恢复点目标 (RPO)：在 GRS 和 RA-GRS 中，存储服务以异步方式将数据从主位置异地复制到辅助位置。 如果发生严重的区域性灾难并且必须执行故障转移，则尚未进行异地复制的最新增量更改可能会丢失。 潜在的数据丢失分钟数称为 RPO（这表示可以将数据恢复到的时间点）。 我们的 RPO 通常小于 15 分钟，但目前没有 SLA 规定异地复制所用时长。

   恢复时间目标 (RTO)：这用来度量必须执行故障转移时花费多长时间来执行故障转移并使存储帐户恢复联机。 执行故障转移的时间包括以下各项：
    * 我们进行调查并决定是可以在主位置恢复数据还是需要执行故障转移所花费的时间。
    * 通过将主 DNS 条目更改为指向辅助位置对帐户进行故障转移。

   我们负责非常认真地保留你的数据，因此，如果有任何机会来恢复数据，我们将延迟执行故障转移并集中精力在主位置恢复数据。 将来，我们计划提供一个 API 来允许你在帐户级别触发故障转移，这将允许你自己控制 RTO，但此功能尚不可用。

## <a name="next-steps"></a>后续步骤
* [使用 RA-GRS 存储设计高度可用的应用程序](../storage-designing-ha-apps-with-ragrs.md)
* [Azure 存储定价](https://www.azure.cn/pricing/details/storage/)
* [关于 Azure 存储帐户](../storage-create-storage-account.md)
* [Azure 存储可伸缩性和性能目标](storage-scalability-targets.md)
* [Azure 存储冗余选项和读取访问异地冗余存储](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)
* [SOSP Paper - Azure Storage: A highly available cloud storage service with strong consistency（SOSP 论文 - Azure 存储：具有高度一致性的高可用云存储服务）](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

<!--Update_Description:wording update-->