---
title: Blob 的 Azure 热、冷存储 | Microsoft Docs
description: 适用于 Azure 存储帐户的热、冷存储。
services: storage
author: WenJason
ms.service: storage
ms.topic: article
origin.date: 09/11/2018
ms.date: 09/24/2018
ms.author: v-jay
ms.openlocfilehash: 3922016cebd4fc521aee2d491fd4529af205ac81
ms.sourcegitcommit: 0081fb238c35581bb527bdd704008c07079c8fbb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523702"
---
# <a name="azure-blob-storage-hot-cool-storage-tiers"></a>Azure Blob 存储：热、冷存储层

## <a name="overview"></a>概述

Azure 存储为 Blob 对象存储提供两个存储层，以便用户根据数据的使用方式最经济高效地存储数据。 Azure **热存储层** 为存储经常访问的数据进行了优化。 Azure 冷存储层为存储不常访问且存储时间至少为 30 天的数据进行了优化。 冷存储层中的数据可容许略低的可用性，但仍需要像热数据一样的高持续性和类似的访问时间以及吞吐量特征。 对于冷数据，略低的可用性 SLA 和较高的访问成本（与热数据相比）对于更低的存储成本而言是可接受的折衷。

如今，存储在云中的数据以指数速度增长。 若要针对不断增加的存储需求来管理成本，可以根据属性（如访问频率和计划保留期）整理数据以优化成本。 存储在云中的数据在其生成方式、处理方式以及在生存期内的访问方式等方面会不相同。 某些数据在其整个生存期中都会受到积极的访问和修改。 某些数据则在生存期早期会受到频繁访问，随着数据变旧，访问会极大地减少。 某些数据在云中保持空闲状态，并且在存储后很少（如果有）被访问。

这些数据访问方案的每一个都受益于针对特定访问模式进行了优化的差异化存储层。 Azure Blob 存储采用热、冷存储层，通过单独的定价模型来满足对差异化存储层的这种需要。

## <a name="storage-accounts-that-support-tiering"></a>支持分层的存储帐户

在 Blob 存储或常规用途 v2 (GPv2) 帐户中，只能将对象存储数据分到热层或冷层。 常规用途 v1 (GPv1) 帐户不支持分层。 不过，客户可以轻松地将其现有的 GPv1 或 Blob 存储帐户转换为 GPv2 帐户，只需在 Azure 门户中单击一下即可。 GPv2 为 Blob、文件和队列提供新的定价结构，还可以访问各种其他的全新存储功能。 另外，以后只对 GPv2 帐户提供某些新功能和价格折扣。 因此，客户应使用 GPv2 帐户进行评估，但只应在查看所有服务的定价以后再使用此类帐户，因为某些工作负荷的价格在 GPv2 中可能比在 GPv1 中更高。 有关详细信息，请参阅 [Azure 存储帐户概述](../common/storage-account-overview.md)。

Blob 存储和 GPv2 帐户在帐户级别公开“访问层”属性，方便你将存储帐户中任何 Blob 的默认存储层指定为热层或冷层，前提是该 Blob 尚未在对象级别设置层。 对于已在对象级别设置该层的对象，不会应用帐户层。 可以随时在这些存储层之间进行切换。

## <a name="hot-access-tier"></a>热访问层

热存储的存储成本高于冷存储，但访问成本最低。 热存储层的示例使用方案包括：

* 处于活跃使用状态或预期会频繁访问（读取和写入）的数据。
* 分阶段进行处理，并最终迁移至冷存储层的数据。

## <a name="cool-access-tier"></a>冷访问层

与热存储相比，冷存储层的存储成本较低，访问成本较高。 此层适用于将要保留在冷层中至少 30 天的数据。 冷存储层的示例使用方案包括：

* 短期备份和灾难恢复数据集。
* 不再经常查看、但访问时应立即可用的较旧的媒体内容。
* 在收集更多数据以便将来处理时需要经济高效地存储的大型数据集。 （例如，长期存储的科学数据、来自制造设施的原始遥测数据）

## <a name="blob-level-tiering"></a>Blob 级别分层

使用 Blob 级分层功能即可通过名为[设置 Blob 层](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier)的单一操作在对象级别更改数据的层。 可以在使用模式更改时轻松地在热、冷层之间更改 Blob 的访问层，而无需在帐户之间移动数据。 所有层更改都会立即发生。

上次 Blob 层更改的时间通过 Blob 属性“访问层更改时间”公开。 可以覆盖热层和冷层中的 Blob，在这种情况下，新的 Blob 会继承被覆盖的 Blob 的层。
所有两个存储层中的 Blob 可以在同一帐户中共存。 如果 Blob 没有显式分配的层，则会从帐户访问层设置推断相应的层。 如果访问层是从帐户推断出来的，则可看到Blob 属性“推断的访问层”已设置为“true”，且 Blob 属性“访问层”与帐户层匹配。 在 Azure 门户中，推断的访问层与 Blob 访问层（例如，热（推断）或冷（推断））一起显示。


### <a name="blob-level-tiering-billing"></a>Blob 级别分层计费

将 Blob 移到更冷的层（热->冷）时，操作按写入目标层的操作计费，具体说来就是按目标层的写入操作次数（以 10,000 次为单位）和数据写入量（以 GB 为单位）收费。 将 Blob 移到更暖的层（冷->热）时，操作按从源层读取来计费，具体说来就是按源层的读取操作次数（以 10,000 次为单位）和数据检索量（以 GB 为单位）收费。

如果将帐户层从热切换为冷，则只按 GPv2 帐户中没有设置层的所有 Blob 的写入操作次数（以 10,000 次为单位）收费。 不会在 Blob 存储帐户中对此更改收费。 如果将 Blob 存储或 GPv2 帐户从冷切换为热，则会按读取操作次数（以 10,000 次为单位）和数据检索量（以 GB 为单位）收费。 也可能还会针对从冷层移出的任何 Blob 收取早期删除费用。

### <a name="cool-early-deletion"></a>“冷”层提前删除

除了按 GB 和按月收费，移到冷层（仅限 GPv2 帐户）中的 Blob 会有一个 30 天的冷层早期删除期限。

## <a name="comparison-of-the-storage-tiers"></a>存储层的比较

下表显示了对热、冷存储层的比较。

| | 热存储层 | 冷存储层 |
| ---- | ----- | ----- |
| **可用性** | 99.9% | 99% |
| **可用性** <br> （RA-GRS 读取）| 99.99% | 99.9% |
| 使用费 | 存储费用较高，访问和事务费用较低 | 存储费用较低，访问和事务费用较高 |
| 最小对象大小 | 不适用 | 不适用 |
| 最短存储持续时间 | 不适用 | 30 天（仅 GPv2） |
| **延迟** <br> （距第一字节时间） | 毫秒 | 毫秒 |
| **可伸缩性和性能目标** | 与通用存储帐户相同 | 与通用存储帐户相同 |

> [!NOTE]
> Blob 存储帐户支持与通用存储帐户相同的性能和可伸缩性目标。 有关详细信息，请参阅 [Azure 存储的可伸缩性和性能目标](../common/storage-scalability-targets.md?toc=%2fstorage%2fblobs%2ftoc.json)。

## <a name="quickstart-scenarios"></a>快速入门方案

本部分使用 Azure 门户演示以下方案：

* 如何更改 GPv2 或 Blob 存储帐户的默认帐户访问层。
* 如何更改 GPv2 或 Blob 存储帐户中 Blob 的层。

### <a name="change-the-default-account-access-tier-of-a-gpv2-or-blob-storage-account"></a>更改 GPv2 或 Blob 存储帐户的默认帐户访问层

1. 登录到 [Azure 门户](https://portal.azure.cn)。

2. 如果要导航到用户的存储帐户，请先选择“所有资源”，然后选择用户的存储帐户。

3. 在“设置”边栏选项卡中，单击“配置”以查看和/或更改帐户配置。

4. 根据需要选择正确的存储层：将“访问层”设置为“冷”或“热”。

5. 单击边栏选项卡顶部的“保存”。

### <a name="change-the-tier-of-a-blob-in-a-gpv2-or-blob-storage-account"></a>更改 GPv2 或 Blob 存储帐户中 Blob 的层。

1. 登录到 [Azure 门户](https://portal.azure.cn)。

2. 若要导航到存储帐户中的 Blob，请依次选择“所有资源”、存储帐户、容器、Blob。

3. 在 Blob 属性边栏选项卡中单击“访问层”下拉菜单以选择“热”、“冷”存储层。

5. 单击边栏选项卡顶部的“保存”。

## <a name="pricing-and-billing"></a>定价和计费

所有存储帐户使用的定价模型都适用于 Blob 存储，具体取决于每个 blob 的层。 请记住以下计费注意事项：

* **存储成本**：除了存储的数据量，存储数据的成本因存储层而异。 层越冷，单 GB 成本越低。
* **数据访问成本**：层越冷，数据访问费用越高。 对于冷层中的数据，需要按 GB 支付读取方面的数据访问费用。
* **事务成本**：层越冷，每个层的按事务收费越高。
* **异地复制数据传输成本**：此费用仅适用于配置了异地复制的帐户，包括 GRS 和 RA-GRS。 异地复制数据传输会产生每 GB 费用。
* **出站数据传输成本**：出站数据传输（传出 Azure 区域的数据）会按每 GB 产生带宽使用费，与通用存储帐户一致。
* **更改存储层**：将帐户存储层从“冷”更改为“热”会产生费用，费用等于读取存储帐户中存在的所有数据的费用。 但是，将帐户存储层从热更改为冷产生的费用则相当于将所有数据写入冷层（仅限 GPv2 帐户）。

> [!NOTE]
> 有关 Blob 存储帐户定价的详细信息，请参阅 [Azure 存储定价](https://azure.cn/pricing/details/storage/)页。 有关出站数据传输收费的详细信息，请参阅[数据传输定价详细信息](https://www.azure.cn/zh-cn/pricing/details/data-transfer/)页。

## <a name="faq"></a>常见问题

**如果要对数据分层，是应该使用 Blob 存储帐户还是 GPv2 帐户？**

建议使用 GPv2 帐户而非 Blob 存储帐户进行分层。 GPv2 支持 Blob 存储帐户支持的所有功能，以及许多其他功能。 Blob 存储和 GPv2 的定价几乎相同，但某些新功能和价格折扣只提供给 GPv2 帐户。 GPv1 帐户不支持分层。

GPv1 和 GPv2 帐户的定价结构不同，客户在决定使用 GPv2 帐户之前，应仔细评估这二者。 只需单击一下，即可轻松地将现有的 Blob 存储或 GPv1 帐户转换为 GPv2 帐户。 有关详细信息，请参阅 [Azure 存储帐户概述](../common/storage-account-overview.md)。

**是否可以将对象存储在同一帐户中的所有两个（热、冷）存储层中？**

是的。 在帐户级别设置的“访问层”属性是一个默认层，适用于该帐户中没有显式设置层的所有对象。 但是，Blob 级别分层允许在对象级别设置访问层，不管该帐户上的访问层设置如何。 这两个存储层（热、冷）中任何一层的 Blob 都可以存在于同一帐户中。

**是否可以更改 Blob 或 GPv2 存储帐户的默认存储层？**

是的，可以通过设置存储帐户上的“访问层”属性来更改默认存储层。 更改存储层适用于存储在帐户中的所有对象，前提是该帐户没有显式设置层。 将存储层从热切换为冷只对 GPv2 帐户中没有设置层的所有 Blob 产生写入操作次数（以 10,000 次为单位）收费，而从冷切换为热则会对 Blob 存储和 GPv2 帐户中的所有 Blob 产生读取操作次数（以 10,000 次为单位）和数据检索量（以 GB 为单位）收费。

**在哪些区域中提供热和冷存储层？**

所有区域均提供热存储层和冷存储层以及 Blob 级别的分层。

冷存储层中 Blob 的行为方式是否与热存储层中的不同？

热存储层中 Blob 的延迟与 GPv1、GPv2 和 Blob 存储帐户中 Blob 的延迟相同。 冷存储层中 Blob 的延迟（以毫秒为单位）与 GPv1、GPv2 和 Blob 存储帐户中 Blob 的延迟类似。 

冷存储层中的 Blob 具有的可用性服务级别 (SLA) 比存储在热存储层中的 Blob 略低。 有关详细信息，请参阅[存储的 SLA](https://www.azure.cn/zh-cn/support/sla/storage/)。

**热层、冷层中的操作是否相同？**

是的。 热层和冷层中的所有操作 100% 一致。 

**设置 Blob 的层以后，何时开始按相应费率收费？**

每个 Blob 始终按“访问层”Blob 属性指示的层收费。 在 Blob 上设置新层时，“访问层”属性将立即反映所有转换的新层。 

**哪些 Azure 工具和 SDK 支持 Blob 级别分层？**

Azure 门户、PowerShell 和 CLI 工具以及 .NET、Java、Python 和 Node.js 客户端库都支持 Blob 级别分层。  

**可以在热层、冷层中存储多少数据？**

数据存储和其他限制在帐户级别设置，不是按存储层设置。 因此，可以选择在一个层中用完所有存储配额，也可以选择在两个层中用完。 有关详细信息，请参阅 [Azure 存储可伸缩性和性能目标](../common/storage-scalability-targets.md?toc=%2fstorage%2fblobs%2ftoc.json)。

## <a name="next-steps"></a>后续步骤

### <a name="evaluate-hot-and-cool-in-gpv2-blob-storage-accounts"></a>评估 GPv2 Blob 存储帐户中的热层和冷层

[按区域查看热层、冷层](https://www.azure.cn/zh-cn/support/service-dashboard/)

[通过启用 Azure 存储度量值来评估当前存储帐户的使用情况](../common/storage-enable-and-view-metrics.md)

[按区域查看 Blob 存储帐户和 GPv2 帐户中的热层、冷层定价](https://www.azure.cn/pricing/details/storage/)

[检查数据传输定价](https://www.azure.cn/pricing/details/data-transfer/)
