---
title: include 文件
description: include 文件
services: container-registry
author: rockboyfor
ms.service: container-registry
ms.topic: include
origin.date: 08/30/2018
ms.date: 11/12/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 6ea12c87c5b0dd904002a9cba804a7756fc3f63a
ms.sourcegitcommit: e8a0b7c483d88bd3c88ed47ed2f7637dec171a17
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2018
ms.locfileid: "51210053"
---
| 资源 | 基本 | 标准 | 高级 |
|---|---|---|---|---|
| 存储<sup>1</sup> | 10 GiB | 100 GiB| 500 GiB |
| 最大映像层大小 | 20 GiB | 20 GiB | 50 GiB |
| 每分钟读取操作数<sup>2、3</sup> | 1,000 | 3,000 | 10,000 |
| 每分钟写入操作数<sup>2、4</sup> | 100 | 500 | 2,000 |
| 下载带宽 (MBps)<sup>2</sup> | 30 | 60 | 100 |
| 上传带宽 (MBps)<sup>2</sup> | 10 个 | 20 | 50 |
| Webhook | 2 | 10 | 100 |

<!-- Not Available on [Supported](/container-registry/container-registry-geo-replication)-->
<!-- Not Available on [Supported][content-trust]-->

<sup>1</sup>指定的存储空间上限是每层的包含的存储空间量。 对于超出这些限制的图像存储，将每日针对每 GiB 进行额外收费。 有关费率的信息，请参阅[容器注册表定价][pricing]。

<sup>2</sup>读取操作数、写入操作数和带宽是最小估计值。 ACR 旨在随使用情况增多提升性能。

<sup>3</sup>[docker pull](https://docs.docker.com/registry/spec/api/#pulling-an-image) 根据映像中的层数和清单检索行为转换为多个读取操作。

<sup>4</sup>[docker push](https://docs.docker.com/registry/spec/api/#pushing-an-image) 根据必须推送的层数转换为多个写入操作。 `docker push` 包含 ReadOps，用于检索现有映像的清单。

<!-- LINKS - External -->
[pricing]: https://www.azure.cn/pricing/details/container-registry/

<!-- LINKS - Internal -->
<!-- Not Available on [geo-replication]: ../articles/container-registry/container-registry-geo-replication.md-->
<!-- Not Available on [content-trust]: ../articles/container-registry/container-registry-content-trust.md-->

<!-- Update_Description: update meta properties  -->