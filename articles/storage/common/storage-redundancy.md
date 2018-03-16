---
title: "Azure 存储中的数据复制 | Microsoft Docs"
description: "复制 Azure 存储帐户中的数据，实现持久性和高可用性。 复制选项包括本地冗余存储 (LRS)、区域冗余存储 (ZRS)、异地冗余存储 (GRS) 和读取访问异地冗余存储 (RA-GRS)。"
services: storage
author: yunan2016
manager: digimobile
ms.service: storage
ms.workload: storage
ms.topic: article
origin.date: 01/21/2018
ms.date: 3/5/2018
ms.author: v-nany
ms.openlocfilehash: 994783e5d01f56754f132f763bb8eec54cb37c61
ms.sourcegitcommit: ad7accbbd1bc7ce0aeb2b58ce9013b7cafa4668b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2018
---
# <a name="azure-storage-replication"></a>Azure 存储复制

始终复制 Microsoft Azure 存储帐户中的数据以确保持久性和高可用性。 复制功能会复制数据，以便可防范临时硬件故障，从而使应用程序保持正常运行。 

可以选择在同一数据中心中、跨同一区域中的数据中心或者跨区域复制数据。 如果跨多个数据中心或者跨区域复制数据，则还可防范单一位置的灾难性故障。

即使面临故障时，复制也可确保存储帐户满足[存储的服务级别协议 (SLA)](https://www.azure.cn/support/sla/storage/)的要求。 请参阅 SLA，了解有关 Azure 存储确保持续性和可用性的信息。

创建存储帐户时，可以选择以下复制选项之一：

* [本地冗余存储 (LRS)](#locally-redundant-storage)
* [异地冗余存储 (GRS)](#geo-redundant-storage)
* [读取访问异地冗余存储 (RA-GRS)](#read-access-geo-redundant-storage)

创建存储帐户时，默认选项为读取访问异地冗余存储 (RA-GRS)。

下表提供 LRS、ZRS、GRS 和 RA-GRS 之间差异的简要概述。 本文的后续各部分更加详细地介绍了每种类型的复制。

| 复制策略 | LRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |
| 数据在多个数据中心之间进行复制。 |否 |是 |是 |
| 可以从辅助位置和主位置读取数据。 |否 |否 |是 |
| 设计为在给定的一年内提供 ___ 对象持久性。 |至少 99.999999999%（11 个 9）|至少 99.99999999999999%（16 个 9）|至少 99.99999999999999%（16 个 9）|

有关不同冗余选项的定价信息，请参阅 [Azure 存储定价](https://www.azure.cn/pricing/details/storage/)。

> [!NOTE]
> 高级存储仅支持本地冗余存储 (LRS)。 有关高级存储的信息，请参阅[高级存储：适用于 Azure 虚拟机工作负荷的高性能存储](../../virtual-machines/windows/premium-storage.md)。
>

## <a name="locally-redundant-storage"></a>本地冗余存储
[!INCLUDE [storage-common-redundancy-LRS](../../../includes/storage-common-redundancy-LRS.md)]


## <a name="geo-redundant-storage"></a>异地冗余存储
[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-GRS.md)]

## <a name="read-access-geo-redundant-storage"></a>读取访问异地冗余存储
读取访问异地冗余存储 (RA-GRS) 可最大程度提高存储帐户的可用性。 除了跨两个区域的异地复制之外，RA-GRS 提供对次要位置中的数据的只读访问。

当启用对次要区域中的数据的只读访问权限时，可以在存储帐户的辅助终结点以及主终结点上获取数据。 辅助终结点与主终结点类似，但会在帐户名称后面追加后缀 `-secondary` 。 例如，如果 Blob 服务的主终结点是 `myaccount.blob.core.chinacloudapi.cn`，则辅助终结点是 `myaccount-secondary.blob.core.chinacloudapi.cn`。 存储帐户的访问密钥对于主终结点和辅助终结点是相同的。

使用 RA-GRS 时需要注意一些注意事项：

* 应用程序必须管理在使用 RA-GRS 时要与哪些终结点进行交互。
* 由于异步复制涉及延迟，因此如果无法将数据从主要区域中恢复（例如在遇到区域性灾难时），则尚未复制到次要区域的更改可能会丢失。
* 如果 Azure 启动了到次要区域的故障转移，则在故障转移完成以后，用户将对该数据具有读取和写入访问权限。 有关详细信息，请参阅[灾难恢复指南](../storage-disaster-recovery-guidance.md)。 
* 要实现高可用性时使用 RA-GRS。 有关可伸缩性的指南，请查看[性能清单](../storage-performance-checklist.md)。

## <a name="frequently-asked-questions"></a>常见问题

<a id="howtochange"></a>
#### <a name="1-how-can-i-change-the-geo-replication-type-of-my-storage-account"></a>1.如何更改存储帐户的异地复制类型？

   可以使用 [Azure 门户](https://portal.azure.cn/)、[Azure Powershell](storage-powershell-guide-full.md) 或者 Azure 存储客户端之一更改存储帐户的异地复制类型。

   > [!NOTE]
   > ZRS 帐户无法转换为 LRS 或 GRS。 同样，现有 LRS 或 GRS 帐户无法转换为 ZRS 帐户。
    
<a id="changedowntime"></a>
#### <a name="2-does-changing-the-replication-type-of-my-storage-account-result-in-down-time"></a>2.更改存储帐户的复制类型是否会导致停机？

   不会，更改存储帐户的复制类型不会导致停机。

<a id="changecost"></a>
#### <a name="3-are-there-additional-costs-to-changing-the-replication-type-of-my-storage-account"></a>3.更改存储帐户的复制类型是否会产生额外费用？

   是的。 如果将存储帐户的复制类型从 LRS 更改为 GRS（或 RA-GRS），则会产生额外的出口流量费用，该费用与将现有数据从主要位置复制到次要位置相关。 复制初始数据后，从主要位置到次要位置进行异地复制不会进一步产生额外的出口流量费用。 有关带宽费用的详细信息，请参阅 [Azure 存储定价页面](https://www.azure.cn/pricing/details/storage)。

   如果从 GRS 更改为 LRS，则不会产生额外的费用，但会从次要位置删除你的数据。

<a id="ragrsbenefits"></a>
#### <a name="4-how-can-ra-grs-help-me"></a>4.RA-GRS 对我有何帮助？

   GRS 存储将数据从主要区域复制到距主要区域数百英里的次要区域。 使用 GRS 时，即使在遇到区域完全停电或导致主要区域不可恢复的灾难，数据也能持久保存。 RA-GRS 存储提供 GRS 复制，还能从次要位置读取数据。 有关如何为实现高可用性进行设计的建议，请参阅[使用 RA-GRS 存储设计高度可用的应用程序](../storage-designing-ha-apps-with-ragrs.md)。

<a id="lastsynctime"></a>
#### <a name="5-is-there-a-way-to-figure-out-how-long-it-takes-to-replicate-my-data-from-the-primary-to-the-secondary-region"></a>5.是否可以估算出将数据从主要区域复制到次要区域需要花费多长时间？

   如果使用的是 RA-GRS 存储，可以查看你的存储帐户的上次同步时间。 上次同步时间是 GMT 日期/时间值。 在上次同步时间之前主要位置的写入内容已成功写入次要位置，这意味着可以从次要位置读取这些内容。 在上次同步时间之后发生的主位置写入可能可供读取，也可能尚不可供读取。 可以使用 [Azure 门户](https://portal.azure.com/)、[Azure PowerShell](storage-powershell-guide-full.md) 或通过 Azure 存储客户端库之一查询此值。

<a id="outage"></a>
#### <a name="6-if-there-is-an-outage-in-the-primary-region-how-do-i-switch-to-the-secondary-region"></a>6.如果主要区域发生中断，如何切换到次要区域？

   有关详细信息，请参阅[在 Azure 存储中断时该怎么办](../storage-disaster-recovery-guidance.md)。

<a id="rpo-rto"></a>
#### <a name="7-what-is-the-rpo-and-rto-with-grs"></a>7.什么是采用 GRS 的 RPO 和 RTO？

   **恢复点目标 (RPO)：**在 GRS 和 RA-GRS 中，存储服务以异步方式将数据从主要位置异地复制到次要位置。 主要区域中发生重大区域灾难时，Microsoft 会故障转移到次要区域。 如果进行故障转移，则尚未进行异地复制的最近更改可能会丢失。 潜在的数据丢失分钟数称为 RPO，它表示可将数据恢复到的时间点。 Azure 存储的 RPO 通常小于 15 分钟，但目前没有 SLA 规定异地复制所用时长。

   **恢复时间目标 (RTO)：**RTO 用于度量执行故障转移以及将存储帐户恢复联机所花费的时间。 执行故障转移的时间包括以下操作：

   * Azure 确定是否可以在主要位置恢复数据或是否需要故障转移所需的时间。
   * 通过将主要 DNS 条目更改为指向次要位置来执行存储帐户故障转移的时间。

   Azure 会认真负责保留数据。 如果有任何机会在主要区域中恢复数据，则 Microsoft 会延迟故障转移并专注于恢复数据。 该服务的未来版本将允许在帐户级别触发故障转移，从而可以自己控制 RTO。

## <a name="next-steps"></a>后续步骤
* [使用 RA-GRS 存储设计高度可用的应用程序](../storage-designing-ha-apps-with-ragrs.md)
* [Azure 存储定价](https://www.azure.cn/pricing/details/storage/)
* [关于 Azure 存储帐户](../storage-create-storage-account.md)
* [Azure 存储可伸缩性和性能目标](storage-scalability-targets.md)
* [Azure 存储冗余选项和读取访问异地冗余存储](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)
* [SOSP Paper - Azure Storage: A highly available cloud storage service with strong consistency（SOSP 论文 - Azure 存储：具有高度一致性的高可用云存储服务）](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

<!--Update_Description:wording update-->