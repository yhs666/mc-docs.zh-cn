---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 01/02/2019
ms.date: 01/21/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: ce497c6e4e857c04ef9ee73712b797548d0f7d52
ms.sourcegitcommit: 317ea7e3b2d307569d3bf7777bd3077013ae4df6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2019
ms.locfileid: "54334504"
---
Azure 存储提供三种类型的存储帐户。 每种类型支持不同的功能，并且具有自己的定价模型。 在创建存储帐户之前，需考虑到这些差异，以便确定最适合应用程序的帐户类型。 这三种类型的存储帐户是：

* **常规用途 v2 帐户**：Blob、文件、队列和表的基本存储帐户类型。 建议在大多数情况下使用 Azure 存储。
* **常规用途 v1 帐户**：Blob、文件、队列和表的旧帐户类型。 如果可能，请改用常规用途 v2 帐户。
* **Blob 存储帐户**：仅限 Blob 的存储帐户。 如果可能，请改用常规用途 v2 帐户。 

下表介绍存储帐户的类型及其功能：

| 存储帐户类型 | 支持的服务                       | 支持的性能层 | 支持的访问层               | 复制选项                                                | 部署模型<sup>1</sup>  | 加密<sup>2</sup> |
|----------------------|------------------------------------------|-----------------------------|--------------------------------------|--------------------------------------------------------------------|-------------------|------------|
| 常规用途 V2   | Blob、文件、队列、表和磁盘       | 标准、高级           | 热、冷 | LRS、GRS、RA-GRS | Resource Manager | 加密  |
| 常规用途 V1   | Blob、文件、队列、表和磁盘       | 标准、高级           | 不适用                                  | LRS、GRS、RA-GRS                                                   | 资源管理器、经典  | 加密  |
| Blob 存储         | Blob（仅块 Blob 和追加 Blob） | 标准                    | 热、冷                            | LRS、GRS、RA-GRS                                                   | Resource Manager  | 加密  |

<sup>1</sup>建议使用 Azure 资源管理器部署模型。 使用经典部署模型的存储帐户仍可在某些位置创建，而现有的经典帐户仍然会受支持。 有关详细信息，请参阅 [Azure 资源管理器与经典部署：了解部署模型和资源状态](../articles/azure-resource-manager/resource-manager-deployment-model.md)。

<sup>2</sup>使用针对静态数据的存储服务加密 (SSE) 来加密所有存储帐户。 有关详细信息，请参阅[静态数据的 Azure 存储服务加密](../articles/storage/common/storage-service-encryption.md)。

