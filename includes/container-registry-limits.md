---
title: include 文件
description: include 文件
services: container-registry
author: rockboyfor
ms.service: container-registry
ms.topic: include
origin.date: 04/29/2019
ms.date: 06/03/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: d3fb75e3e1ac324d05df983a6a66ad3b2f339c8e
ms.sourcegitcommit: d75eeed435fda6e7a2ec956d7c7a41aae079b37c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66195451"
---
| 资源 | 基本 | 标准 | 高级 |
|---|---|---|---|---|
| 存储<sup>1</sup> | 10 GiB | 100 GiB| 500 GiB |
| 最大映像层大小 | 200 GiB | 200 GiB | 200 GiB |
| 每分钟读取操作数<sup>2、3</sup> | 1,000 | 3,000 | 10,000 |
| 每分钟写入操作数<sup>2、4</sup> | 100 | 500 | 2,000 |
| 下载带宽 (MBps)<sup>2</sup> | 30 | 60 | 100 |
| 上传带宽 (MBps)<sup>2</sup> | 10 个 | 20 | 50 |
| Webhook | 2 | 10 | 100 |
| 异地复制 | 不适用 | 不适用 | [受支持][geo-replication] |

<!-- Not Available on [Supported][content-trust]-->

<sup>1</sup> 指定的存储上限是每层包含  的存储量。 对于超出这些限制的图像存储，将每日针对每 GiB 进行额外收费。 有关费率的信息，请参阅 [Azure 容器注册表定价][pricing]。

<sup>2</sup>*读取操作数*、*写入操作数*和*带宽*是最小估计值。 Azure 容器注册表根据使用需求努力提高性能。

<sup>3</sup>[docker pull](https://docs.docker.com/registry/spec/api/#pulling-an-image) 根据映像中的层数和清单检索行为转换为多个读取操作。

<sup>4</sup>[docker push](https://docs.docker.com/registry/spec/api/#pushing-an-image) 根据必须推送的层数转换为多个写入操作。 `docker push` 包含 ReadOps  ，用于检索现有映像的清单。

<!-- LINKS - External -->
[pricing]: https://www.azure.cn/pricing/details/container-registry/

<!-- LINKS - Internal -->
[geo-replication]: ../articles/container-registry/container-registry-geo-replication.md

<!-- Not Available on [content-trust]: ../articles/container-registry/container-registry-content-trust.md-->

<!-- Update_Description: update meta properties  -->