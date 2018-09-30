---
title: Azure 容器注册表中的映像存储
description: 详述如何在 Azure 容器注册表中存储 Docker 容器映像，包括安全性、冗余和容量。
services: container-registry
author: rockboyfor
manager: digimobile
ms.service: container-registry
ms.topic: article
origin.date: 03/21/2018
ms.date: 09/30/2018
ms.author: v-yeche
ms.openlocfilehash: 4c86722fb0d9de1ef42ccf07c1f4259f158c5113
ms.sourcegitcommit: 7aa5ec1a312fd37754bf17a692605212f6b716cd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/26/2018
ms.locfileid: "47201253"
---
# <a name="container-image-storage-in-azure-container-registry"></a>Azure 容器注册表中的容器映像存储

每个[基本、标准和高级](container-registry-skus.md) Azure 容器注册表均受益于高级 Azure 存储功能，如静态加密（以确保映像数据安全）和地域冗余（以实现映像数据保护）。 以下部分介绍 Azure 容器注册表 (ACR) 中映射存储的功能和限制。

## <a name="encryption-at-rest"></a>静态加密

注册表中的所有容器映像均已进行静态加密。 Azure 在存储映像之前自动对其进行加密，当应用程序和服务请求映像时即时对其进行解密。

## <a name="geo-redundant-storage"></a>异地冗余存储

Azure 使用异地冗余存储方案来防止容器映像丢失。 Azure 容器注册表会自动将容器映像复制到多个地理位置相距遥远的数据中心，以防止其在区域存储失败时丢失。

<!-- Not Available on ## Geo-replication-->
## <a name="image-limits"></a>映像限制

下表介绍了针对 Azure 容器注册表设置的容器映像和存储限制。

| 资源 | 限制 |
| -------- | :---- |
| 存储库 | 无限制 |
| 映像 | 无限制 |
| 层 | 无限制 |
| 标记 | 无限制|
| 存储 | 5 TB |

大量的存储库和标记可能会影响注册表的性能。 作为注册表维护例程的一部分，定期删除未使用的存储库、标记和图像。 已删除的注册表资源（如存储库、映像和标记）在删除后*无法*恢复。 有关删除注册表资源的详细信息，请参阅[删除 Azure 容器注册表中的容器映像](container-registry-delete.md)。

## <a name="storage-cost"></a>存储成本

有关定价的完整详细信息，请参阅 [Azure 容器注册表定价][pricing]。

## <a name="next-steps"></a>后续步骤

有关不同 Azure 容器注册表 SKU（基本、标准和高级）的详细信息，请参阅 [Azure 容器注册表 SKU](container-registry-skus.md)。

<!-- IMAGES -->

<!-- LINKS - External -->
[portal]: https://portal.azure.cn
[pricing]: http://aka.ms/acr/pricing

<!-- LINKS - Internal -->
<!-- Update_Description: update meta properties, wording update -->
