---
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 11/08/2019
ms.date: 11/25/2019
ms.author: v-jay
ms.openlocfilehash: 4d94444d0dcf74bc799cdf418d72726d754260f5
ms.sourcegitcommit: 6a19227dcc0c6e0da5b82c4f69d0227bf38a514a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74328750"
---
| Resource | 目标        |
|----------|---------------|
| 单个 Blob 容器的最大大小 | 与最大存储帐户容量相同 |
| 块 Blob 或附加 Blob 中的块数上限 | 50,000 块 |
| 块 Blob 中块的最大大小 | 100 MiB |
| 块 Blob 的最大大小 | 50,000 X 100 MiB（大约 4.75 TiB） |
| 附加 Blob 中块的最大大小 | 4 MiB |
| 附加 Blob 的最大大小 | 50,000 x 4 MiB（大约 195 GiB） |
| 页 Blob 的最大大小 | 8 TiB |
| 每个 Blob 容器存储的访问策略的最大数目 | 5 |
|单个 blob 的目标请求速率 | 每秒最多 500 个请求 |
|单个页 blob 的目标吞吐量 | 最高每秒 60 MiB |
|单个块 blob 的目标吞吐量 |上限为存储帐户的传入/传出限制<sup>1</sup> |

<sup>1</sup> 单一对象吞吐量取决于多个因素，包括但不限于：并发性、请求大小、性能层、源上传速度和目标下载速度。 若要利用[高吞吐量块 Blob](https://azure.microsoft.com/blog/high-throughput-with-azure-blob-storage/) 性能增强功能，请使用 4 MiB 以上（Data Lake Storage Gen2 则为 256 KiB 以上）的放置 Blob 或放置块请求大小。