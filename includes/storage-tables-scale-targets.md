---
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 05/29/2019
ms.date: 07/15/2019
ms.author: v-jay
ms.openlocfilehash: 9332949a4ebe23528f81295cb7814b8e1defcdae
ms.sourcegitcommit: 80336a53411d5fce4c25e291e6634fa6bd72695e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/12/2019
ms.locfileid: "67844455"
---
| Resource | 目标 |
|----------|---------------|
| 单个表的最大大小 | 500 TiB |
| 表实体的最大大小 | 1 MiB |
| 表实体中属性的最大数目 | 255，包含三个系统属性：PartitionKey、RowKey 和 Timestamp |
| 实体中属性值的最大总大小 | 1 MiB |
| 每个表存储的访问策略的最大数目 | 5 |
| 每个存储帐户的最大请求速率 | 20,000 事务/秒，假定实体大小为 1-KiB |
| 单个表分区的目标吞吐量（1 KiB 实体） | 每秒最多 2,000 个实体 |
