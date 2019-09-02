---
author: WenJason
ms.service: databox
ms.subservice: pod
ms.topic: include
origin.date: 05/22/2019
ms.date: 06/10/2019
ms.author: v-jay
ms.openlocfilehash: 453473926271f2f19e57151077ab53aca3d26e90
ms.sourcegitcommit: 67a78cae1f34c2d19ef3eeeff2717aa0f78de38e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2019
ms.locfileid: "66726511"
---
下面是 Data Box 设备支持的存储帐户和存储类型的列表。 有关所有不同类型的存储帐户及其完整功能的完整列表，请参阅[存储帐户类型](/storage/common/storage-account-overview#types-of-storage-accounts)。

| **存储帐户/支持的存储类型** | **块 blob** |**页 blob*** |**Azure 文件存储** |**说明**|
| --- | --- | -- | -- | -- |
| 经典标准 | Y | Y | Y |
| 常规用途 v1 标准  | Y | Y | Y | 支持热和冷。|
| 常规用途 v1 高级  |  | Y| | |
| 常规用途 v2 标准  | Y | Y | Y | 支持热和冷。|
| 常规用途 v2 高级  |  |Y | | |
| Blob 存储标准 |Y | | |支持热和冷。 |

\* *- 上传到页 blob 的数据必须是 512 字节对齐，例如 VHD。*

>[!NOTE]
> 不支持 Azure Data Lake Storage Gen 2 帐户。
