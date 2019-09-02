---
author: rockboyfor
ms.service: container-registry
ms.topic: include
origin.date: 11/09/2018
ms.date: 12/17/2018
ms.author: v-yeche
ms.openlocfilehash: 90502f396b8521ac8fca44827b88525d7f0a3c76
ms.sourcegitcommit: 1db6f261786b4f0364f1bfd51fd2db859d0fc224
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2018
ms.locfileid: "53286783"
---
| SKU | 说明 |
|---|---|
| 基本 | 供开发者了解 Azure 容器注册表的入口点（已优化过成本）。 基本注册表的编程功能（Azure Active Directory 身份验证集成、映像删除和 Webhook）与标准注册表和高级注册表相同。不同之处在于大小和使用情况约束。 |
| 标准 | 标准注册表的功能与基本注册表相同。不同之处在于，前者增加了存储空间上限和映像吞吐量。 标准注册表应能够满足大部分生产方案的需求。 |
| 高级 | 高级注册表对存储和并发操作等功能的约束限制更高，包括为了支持大容量方案而增强存储功能。 除了增加映像吞吐容量之外，高级注册表还增添了其他功能（如在每个部署中维护网络封闭注册表）。 |

<!-- Not Available on Line 13 geo-replication for managing a single registry across multiple regions-->
