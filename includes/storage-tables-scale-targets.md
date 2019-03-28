---
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 10/26/2018
ms.date: 03/25/2019
ms.author: v-jay
ms.openlocfilehash: bf82242d7fd5caa19e887033cb09dabcdc08ec62
ms.sourcegitcommit: c70402dacd23ccded50ec6aea9f27f1cf0ec22ba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2019
ms.locfileid: "58261570"
---
| 资源 | 目标 |
|----------|---------------|
| 单个表的最大大小 | 500 TiB |
| 表实体的最大大小 | 1 MiB |
| 表实体中属性的最大数目 | 255，包含三个系统属性：PartitionKey、RowKey 和 Timestamp |
| 每个表存储的访问策略的最大数目 | 5 |
| 每个存储帐户的最大请求速率 | 20,000 事务/秒，假定实体大小为 1-KiB |
| 单个表分区的目标吞吐量（1 KiB 实体） | 每秒最多 2,000 个实体 |
