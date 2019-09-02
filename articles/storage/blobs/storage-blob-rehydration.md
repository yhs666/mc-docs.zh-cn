---
title: 从存档层解冻 Blob 数据
description: 从存档存储中解冻 Blob，以便可以访问数据。
services: storage
author: WenJason
ms.author: v-jay
origin.date: 08/07/2019
ms.date: 08/19/2019
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.reviewer: hux
ms.openlocfilehash: 956b9979045beddfd393fc0beda687e03e4409fa
ms.sourcegitcommit: 0de1021cff162a602777858b3f0b7949557fd22c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2019
ms.locfileid: "69014636"
---
# <a name="rehydrate-blob-data-from-the-archive-tier"></a>从存档层解冻 Blob 数据

当 Blob 位于存档访问层时，它被视为脱机状态，无法对其进行读取或修改。 Blob 元数据保持联机和可用状态，你可以列出 Blob 及其属性。 只能读取和修改联机层（例如热层或冷层）中的 Blob 数据。 可使用两个选项来检索和访问存档访问层中存储的数据。

1. [将存档的 Blob 解冻到联机层](#rehydrate-an-archived-blob-to-an-online-tier) - 使用[设置 Blob 层](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier)操作将存档的 Blob 解冻至热层或冷层。
2. [将存档的 Blob 复制到联机层](#copy-an-archived-blob-to-an-online-tier) - 使用[复制 Blob](https://docs.microsoft.com/rest/api/storageservices/copy-blob) 操作创建已存档 Blob 的新副本。 指定不同的 Blob 名称，以及目标热层或冷层。

 有关层的详细信息，请参阅 [Azure Blob 存储：热、冷和存档访问层](storage-blob-storage-tiers.md)。

## <a name="rehydrate-an-archived-blob-to-an-online-tier"></a>将存档的 Blob 解冻到联机层

[!INCLUDE [storage-blob-rehydration](../../../includes/storage-blob-rehydrate-include.md)]

## <a name="copy-an-archived-blob-to-an-online-tier"></a>将存档的 Blob 复制到联机层

如果你不想要解冻 Blob，可以选择[复制 Blob](https://docs.microsoft.com/rest/api/storageservices/copy-blob) 操作。 原始 Blob 在存档中保持未修改状态，同时，你可以在热层或冷层中处理新的 Blob。 使用复制过程时，可将可选的 *x-ms-rehydrate-priority* 属性设置为 Standard（标准）或 High（高）（预览版功能）。

只能将存档 Blob 复制到联机目标层。 不支持将存档 Blob 复制到另一个存档 Blob。

从存档中复制 Blob 需要花费一定的时间。 在幕后，“复制 Blob”操作会暂时解冻存档源 Blob，以便在目标层中创建新的联机 Blob。  在完成从存档中暂时解冻并将数据写入新 Blob 之前，此新 Blob 不可用。

## <a name="pricing-and-billing"></a>定价和计费

将存档中的 Blob 解冻到热层或冷层会产生读取操作和数据检索费用。 与使用标准优先级相比，使用高优先级（预览版功能）产生的操作和数据检索费用更高。 高优先级解冻在帐单上单独显示费用细目。 如果返回若干 GB 存档 Blob 的高优先级请求花费了 5 小时以上，则不会按照高优先级检索费率向你收费。 不过，标准检索费率仍适用。

将存档中的 Blob 复制到热层或冷层会产生读取操作和数据检索费用。 创建新副本会产生写入操作费用。 复制到联机 Blob 时，不会产生提前删除费，因为源 Blob 在存档层中保持未修改状态。 高优先级费用适用。

存档层中的 Blob 应至少存储 180 天。 在 180 天之前删除或解冻存档的 Blob 会导致提前删除费。

> [!NOTE]
> 有关块 Blob 和数据解冻的定价详细信息，请参阅 [Azure 存储定价](https://azure.cn/pricing/details/storage/blobs/)。 有关出站数据传输费的详细信息，请参阅[数据传输定价详细信息](https://www.azure.cn/pricing/details/bandwidth/)。

## <a name="next-steps"></a>后续步骤

* [了解 Blob 存储层](storage-blob-storage-tiers.md)
* [按区域查看 Blob 存储帐户和 GPv2 帐户中的热层、冷层和存档层定价](https://azure.cn/pricing/details/storage/)
* [管理 Azure Blob 存储生命周期](storage-lifecycle-management-concepts.md)
* [检查数据传输定价](https://www.azure.cn/pricing/details/bandwidth/)
