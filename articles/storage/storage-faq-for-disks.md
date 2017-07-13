---
title: "有关 Azure IaaS VM 磁盘的常见问题 (FAQ) | Azure"
description: "有关 Azure IaaS VM 磁盘和高级磁盘（托管与非托管）的常见问题"
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: e2a20625-6224-4187-8401-abadc8f1de91
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/15/2017
ms.date: 06/26/2017
ms.author: v-johch
ms.openlocfilehash: eb9087c779b85e4f0fa28d99e87f3f2ab4764c2c
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
<a id="frequently-asked-questions-about-azure-iaas-vm-disks-and-managed-and-unmanaged-premium-disks" class="xliff"></a>

# 有关 Azure IaaS VM 磁盘以及托管和非托管高级磁盘的常见问题

本文提供有关托管磁盘和高级存储的一些常见问题的解答。

##<a name="managed-disks-and-port-8443"></a> 托管磁盘

**什么是 Azure 托管磁盘？**

托管磁盘是一种通过处理存储帐户管理来简化 Azure IaaS VM 的磁盘管理的功能。 有关详细信息，请参阅[托管磁盘概述](storage-managed-disks-overview.md)。

**如果从现有的 VHD（大小为 80 GB）创建标准托管磁盘，需要多少费用？**

从 80 GB VHD 创建的标准托管磁盘被视为下一个可用的标准磁盘大小（S10 磁盘）。 我们将根据 S10 磁盘定价收费。 有关详细信息，请查看[定价页](https://www.azure.cn/pricing/details/storage)。

**标准托管磁盘是否产生任何事务成本？**

是的，我们会按每个事务收费。 有关详细信息，请查看[定价页] (https://www.azure.cn/pricing/details/storage)。

**对于标准托管磁盘，是对磁盘上的数据实际大小收费还是对磁盘的预配容量收费？**

根据磁盘的预配容量收费。 有关详细信息，请查看[定价页](https://www.azure.cn/pricing/details/storage)。

**高级托管磁盘与非托管磁盘的定价有何不同？**

高级托管磁盘的定价与高级非托管磁盘的定价相同。

是否可以更改托管磁盘的存储帐户类型（标准/高级）？

是的。 可以使用 Azure 门户、PowerShell 或 Azure CLI 更改托管磁盘的存储帐户类型。

**是否可将托管磁盘复制或导出到专用存储帐户？**

是的，可以通过 Azure 门户、PowerShell 或 Azure CLI 导出托管磁盘。

**是否可以使用 Azure 存储帐户中的 VHD 文件在不同的订阅中创建托管磁盘？**

否。

**是否可以使用 Azure 存储帐户中的 VHD 文件在不同的区域中创建托管磁盘？**

否。

**客户使用托管磁盘是否存在任何规模限制？**

托管磁盘取消了与存储帐户相关的限制。 但是，每个订阅的托管磁盘数默认限制为 2000 个。 可联系支持人员提高此限额。

**是否可以创建托管磁盘的增量快照？**

否。 当前的快照功能提供托管磁盘的完整副本。 但我们计划在将来支持增量快照。

**可用性集中的 VM 是否可以同时包含托管和非托管磁盘？**

不可以，可用性集中的 VM 必须全部使用托管磁盘或全部使用非托管磁盘。 创建可用性集时，可以选择要使用的磁盘类型。

**托管磁盘是否是 Azure 门户中的默认选项？**

目前不是，但将来会成为默认选项。

**是否可以创建一个空托管磁盘？**

是的，可以创建空磁盘。 可独立于 VM 创建托管磁盘，也就是说不需要将磁盘附加到 VM。

**什么是使用托管磁盘的可用性集的受支持容错域计数？**

使用托管磁盘的可用性集的受支持容错域计数为 2 或 3，具体取决于它所在的区域。

**如何设置用于诊断的标准存储帐户？**

设置 VM 诊断的专用存储帐户。 我们计划将来也将诊断切换到托管磁盘。

**托管磁盘提供哪种类型的 RBAC 支持？**
托管磁盘支持三个密钥默认角色：

1.  所有者：可管理所有内容，包括访问权限。

2.  参与者：可管理访问权限以外的所有内容。

3.  读取者：可查看所有内容，但不能进行更改。

**是否可将托管磁盘复制或导出到专用存储帐户？**

可以为托管磁盘获取只读共享访问签名 (SAS) URI，使用它将内容复制到专用存储帐户或本地存储。

**是否可以创建托管磁盘副本？**

客户可以生成托管磁盘的快照，然后使用快照创建另一个托管磁盘。

**是否仍支持非托管磁盘？**

是，我们支持托管磁盘和非托管磁盘。 但建议对新的工作负荷使用托管磁盘，并将当前的工作负荷迁移到托管磁盘。

如果创建 128 GB 大小的磁盘，然后将大小增加到 130 GB，是否会针对下一磁盘大小 (512 GB) 进行收费？

是的。

**是否可以创建 LRS、GRS 和 ZRS 托管磁盘？**

Azure 托管磁盘当前仅支持本地冗余存储 (LRS)。

能否收缩/缩小托管磁盘？
否。 目前，不支持此功能。 

在使用专用（未经过 sysprep 处理或通用化）OS 磁盘预配 VM 时能否更改计算机名称属性？否。 无法更新计算机名称属性。 新 VM 将从创建 OS 磁盘时所用的父 VM 继承该属性。 


**在哪里可找到用于使用托管磁盘创建 VM 的示例 Azure Resource Manager 模板**
* https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md
* https://github.com/chagarw/MDPP

<a id="managed-disks-and-storage-service-encryption-sse" class="xliff"></a>

## 托管磁盘和存储服务加密 (SSE)

**创建新的托管磁盘时是否会默认启用 SSE？**

是

**加密密钥由谁管理？**

密钥由 Azure 管理。

**是否可以从我的托管磁盘禁用 SSE？**

否

**SSE 是否仅在特定的区域中可用？**

不是。 它在提供了托管磁盘的所有区域中都可用。 

**如何查明我的托管磁盘是否已加密？**

可以通过 Azure 门户、CLI 和 PowerShell 查找托管磁盘的创建时间。  

**如何加密我的现有磁盘？**

写入到现有托管磁盘中的新数据将自动加密。 我们还计划对现有数据进行加密，并且加密将在后台以异步方式进行。 如果必须现在对现有数据进行加密，则解决方法是创建磁盘的副本。 新磁盘将被加密。

[使用 CLI 复制托管磁盘](https://docs.azure.cn/zh-cn/storage/scripts/storage-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription?toc=%2fcli%2fmodule%2ftoc.json)

[使用 PowerShell 复制托管磁盘](https://docs.azure.cn/zh-cn/storage/scripts/storage-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription?toc=%2fcli%2fmodule%2ftoc.json)

**托管快照和映像是否加密？**

是的。

**如果 VM 中包含目前加密的或之前已加密的存储帐户上的非托管磁盘，是否可以将该 VM 转换为托管磁盘？**

不可以。 尚不支持此功能。 

**从托管磁盘或快照导出的 VHD 是否也是加密的？**

不是。 但是，如果将 VHD 从加密的托管磁盘或快照导出到加密的存储帐户，则会对其进行加密。 


<a id="premium-disks--both-managed-and-unmanaged" class="xliff"></a>

## 高级磁盘 – 托管和非托管

**如果 VM 使用支持高级存储的大小系列（比如 DSv2），是否可以同时附加高级和标准数据磁盘？** 

是的。

**是否可以同时将高级和标准数据磁盘附加到不支持高级存储的大小系列，例如 D、Dv2、G 或 F 系列？**

否。 只能将标准数据磁盘附加到不使用支持高级存储的大小系列的 VM。

**如果从现有的 VHD（大小为 80 GB）创建高级数据磁盘，需要多少费用？**

从 80 GB VHD 创建的高级数据磁盘被视为下一个可用的高级磁盘大小（P10 磁盘）。 我们将根据 P10 磁盘价格收费。

**使用高级存储是否产生事务成本？**

每个磁盘大小都有固定成本，其根据 IOPS 和吞吐量的特定限制进行预配。 其他成本包括出站带宽和快照容量（如果适用）。 有关详细信息，请查看[定价页](https://www.azure.cn/pricing/details/storage/)。

**可从磁盘缓存获取的 IOPS 和吞吐量限制是什么？**

DS 系列的缓存和本地 SSD 合并限制是每个核心 4000 IOPS，以及每个核心每秒 33 MB。 

**托管磁盘 VM 是否支持本地 SSD？**

本地 SSD 是托管磁盘 VM 随附的临时存储。 临时存储不需要额外的成本。 建议不要使用此本地 SSD 来存储应用程序数据，因为这些数据不会永久保存在 Azure Blob 存储中。

在高级磁盘上使用 TRIM 是否有任何影响？

在高级或标准磁盘的 Azure 磁盘上使用 TRIM 没有负面影响。

<a id="new-disk-sizes---both-managed-and-unmanaged" class="xliff"></a>

## 新磁盘大小 - 托管和非托管

**对于 OS 磁盘和数据磁盘，支持的最大磁盘大小是多少？**

对于 OS 磁盘，Azure 支持的分区类型是 MBR（主引导记录）。 MBR 格式支持的最大磁盘大小为 2TB。 因此，Azure 支持的最大 OS 磁盘为 2TB。 对于数据磁盘，Azure 最大支持 4TB。 

**支持的最大页面 blob 大小是多少？**

Azure 支持的最大页面 blob 大小是 8TB (8191GB)。 不支持将大于 4TB (4095GB) 的任何页面 blob 作为数据磁盘或 OS 磁盘附加到 VM。

**若要创建、附加、上传大于 1TB 的磁盘或调整其大小，是否需要使用新版本的 Azure 工具？**

不需要升级现有 Azure 工具即可创建、附加大于 1TB 的磁盘或调整其大小。 如果希望直接将 VHD 文件从本地作为页面 blob/非托管磁盘上传到 Azure， 需要选取下面列出的最新工具集。

|Azure 工具      | 支持的版本                                |
|-----------------|---------------------------------------------------|
|Azure Powershell | 版本号 v4.1.0 – 2017 年 6 月发行版或更高版本|
|Azure CLI v1     | 版本号 0.10.13 – 2017 年 5 月发行版或更高版本|
|AzCopy           | 版本号 v6.1.0 – 2017 年 6 月发行版或更高版本|

即将支持 Azure CLI v2 和存储资源管理器。 

**对于非托管磁盘或页面 Blob，是否支持 P4 和 P6 磁盘大小？**

否，只有对于托管磁盘才支持 P4(32GB) 和 P6(64GB) 磁盘大小。 即将支持非托管磁盘和页面 Blob。

**对于在启用小型磁盘之前（大约 6 月 15 日）创建的小于 64 GB 的现有高级托管磁盘是如何计费的？**

小于 64 GB 的现有小型高级磁盘将继续按 P10 定价层进行计费。 

**如何可以将小于 64 GB 的小型高级磁盘的磁盘层从 P10 切换到 P4 或 P6？**

可以创建小型磁盘的快照，然后创建一个磁盘，该磁盘将根据预配的大小自动将定价层切换到 P4 或 P6。 


<a id="what-if-my-question-isnt-answered-here" class="xliff"></a>

## 如果未在此处找到相关问题怎么办？

如果未在此处找到相关问题，请联系我们，我们会帮助你找到答案。 可将问题发布在本文末尾的评论中或 MSDN [Azure 存储论坛](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata) 中，围绕本文内容，与 Azure 存储团队和其他社区成员展开探讨。

