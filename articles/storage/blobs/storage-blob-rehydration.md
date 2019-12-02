---
title: 从存档层解冻 Blob 数据
description: 从存档存储中解冻 Blob，以便可以访问数据。
services: storage
author: WenJason
ms.author: v-jay
origin.date: 11/14/2019
ms.date: 11/25/2019
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.reviewer: hux
ms.openlocfilehash: 271e72a06a7c50f56220f9737fbb418bdeb6ee6c
ms.sourcegitcommit: 6a19227dcc0c6e0da5b82c4f69d0227bf38a514a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74328651"
---
# <a name="rehydrate-blob-data-from-the-archive-tier"></a>从存档层解冻 Blob 数据

当 Blob 位于存档访问层时，它被视为脱机状态，无法对其进行读取或修改。 Blob 元数据保持联机和可用状态，你可以列出 Blob 及其属性。 只能读取和修改联机层（例如热层或冷层）中的 Blob 数据。 可使用两个选项来检索和访问存档访问层中存储的数据。

1. [将存档的 Blob 解冻到联机层](#rehydrate-an-archived-blob-to-an-online-tier) - 使用[设置 Blob 层](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier)操作将存档的 Blob 解冻至热层或冷层。
2. [将存档的 Blob 复制到联机层](#copy-an-archived-blob-to-an-online-tier) - 使用[复制 Blob](https://docs.microsoft.com/rest/api/storageservices/copy-blob) 操作创建已存档 Blob 的新副本。 指定不同的 Blob 名称，以及目标热层或冷层。

 有关层的详细信息，请参阅 [Azure Blob 存储：热、冷和存档访问层](storage-blob-storage-tiers.md)。

## <a name="rehydrate-an-archived-blob-to-an-online-tier"></a>将存档的 Blob 解冻到联机层

[!INCLUDE [storage-blob-rehydration](../../../includes/storage-blob-rehydrate-include.md)]

## <a name="copy-an-archived-blob-to-an-online-tier"></a>将存档的 Blob 复制到联机层

如果你不想要解冻存档的 Blob，可以选择执行[复制 Blob](https://docs.microsoft.com/rest/api/storageservices/copy-blob) 操作。 原始 Blob 在存档中保持未修改状态，同时会在热层或冷层中联机创建新的 Blob 供你使用。 在“复制 Blob”操作中，也可将可选的 *x-ms-rehydrate-priority* 属性设置为 Standard 或 High（预览版），以便指定创建 blob 副本的优先级。

只能将存档 Blob 复制到同一存储帐户中的联机目标层。 不支持将存档 Blob 复制到另一个存档 Blob。

从存档中复制 Blob 可能需要数小时才能完成，具体取决于所选解冻优先级。 在幕后，“复制 Blob”操作会读取存档源 Blob，以便在所选目标层中创建新的联机 Blob。  列出 Blob 时，新 Blob 也许可见，但数据并不可用，直到从源存档 Blob 进行读取的操作完成并将数据写入到新的联机目标 Blob 为止。 新 Blob 充当独立的副本，对它进行的任何修改或删除操作不会影响源存档 Blob。

## <a name="pricing-and-billing"></a>定价和计费

将存档中的 Blob 解冻到热层或冷层会产生读取操作和数据检索费用。 与使用标准优先级相比，使用高优先级（预览版功能）产生的操作和数据检索费用更高。 高优先级解冻在账单上单独显示费用细目。 如果返回若干 GB 存档 Blob 的高优先级请求花费了 5 小时以上，则不会按照高优先级检索费率向你收费。 不过，标准检索费率仍适用，因为解冻的优先级高于其他请求。

将存档中的 Blob 复制到热层或冷层会产生读取操作和数据检索费用。 创建新的 Blob 副本会产生写入操作费用。 复制到联机 Blob 时，不会产生提前删除费，因为源 Blob 在存档层中保持未修改状态。 高优先级检索费用适用（如果已选中）。

存档层中的 Blob 应至少存储 180 天。 在 180 天之前删除或解冻存档的 Blob 会导致提前删除费。

> [!NOTE]
> 有关块 Blob 和数据解冻的定价详细信息，请参阅 [Azure 存储定价](https://azure.cn/pricing/details/storage/blobs/)。 有关出站数据传输费的详细信息，请参阅[数据传输定价详细信息](https://www.azure.cn/pricing/details/bandwidth/)。

## <a name="next-steps"></a>后续步骤

* [了解 Blob 存储层](storage-blob-storage-tiers.md)
* [按区域查看 Blob 存储帐户和 GPv2 帐户中的热层、冷层和存档层定价](https://azure.cn/pricing/details/storage/)
* [管理 Azure Blob 存储生命周期](storage-lifecycle-management-concepts.md)
* [检查数据传输定价](https://www.azure.cn/pricing/details/bandwidth/)
