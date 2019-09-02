---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
origin.date: 11/09/2018
ms.date: 03/21/2019
ms.author: v-junlch
ms.openlocfilehash: 5ed37fa5f46cbbca856b390c06b67a428b50160f
ms.sourcegitcommit: 02c8419aea45ad075325f67ccc1ad0698a4878f4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319734"
---
| 资源 | 限制 |
| --- | --- |
| 缓存大小 |530 GB |
| 数据库 |64 |
| 连接的最大客户端数 |40,000 |
| Azure Redis 缓存副本，用于高可用性 |1 |
| 启用群集的高级缓存中的分片数 |10 个 |

每个定价层的 Azure Redis 缓存限制和大小都不相同。 若要查看定价层及其关联的大小，请参阅 [Azure Redis 缓存定价](https://www.azure.cn/pricing/details/redis-cache/)。

有关 Azure Redis 缓存配置限制的详细信息，请参阅[默认 Redis 服务器配置](../articles/azure-cache-for-redis/cache-configure.md#default-redis-server-configuration)。

由于 Azure Redis 缓存实例的配置和管理是由 Microsoft 执行的，因此 Azure Redis 缓存并非支持所有 Redis 命令。 有关详细信息，请参阅 [Azure Redis 缓存不支持的 Redis 命令](../articles/azure-cache-for-redis/cache-configure.md#redis-commands-not-supported-in-azure-cache-for-redis)。


