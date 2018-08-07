---
title: Azure SQL 数据库基于 vCore 的资源限制 - 单一数据库 | Azure
description: 本页介绍 Azure SQL 数据库的单一数据库中一些常见的基于 vCore 的资源限制。
services: sql-database
author: WenJason
manager: dogimobile
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
origin.date: 07/16/2018
ms.date: 08/06/2018
ms.author: v-jay
ms.openlocfilehash: a2f9e4e96da47d89adead9abbacb84ffa44e0556
ms.sourcegitcommit: 7ea906b9ec4f501f53b088ea6348465f31d6ebdc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/03/2018
ms.locfileid: "39486845"
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-for-a-single-database"></a>适用于单一数据库的 Azure SQL 数据库基于 vCore 的购买模型限制

本文提供针对使用基于 vCore 的购买模型的 Azure SQL 数据库的单一数据库的详细资源限制。

有关基于 DTU 的购买模型的限制，请参阅 [SQL 数据库基于 DTU 的资源限制](sql-database-dtu-resource-limits.md)。

## <a name="single-database-storage-sizes-and-performance-levels"></a>单一数据库：存储大小和性能级别

对于单一数据库，下表显示了可用于每个服务层和性能级别的单一数据库的资源。 可通过 [Azure 门户](sql-database-servers-databases-manage.md#azure-portal-manage-logical-servers-and-databases)、[Transact-SQL](sql-database-servers-databases-manage.md#transact-sql-manage-logical-servers-and-databases)、[PowerShell](sql-database-servers-databases-manage.md#powershell-manage-logical-servers-and-databases)、[Azure CLI](sql-database-servers-databases-manage.md#azure-cli-manage-logical-servers-and-databases) 或 [REST API](sql-database-servers-databases-manage.md#rest-api-manage-logical-servers-and-databases) 为单一数据库设置服务层、性能级别和存储量。

### <a name="general-purpose-service-tier"></a>常规用途服务层

#### <a name="generation-4-compute-platform"></a>第 4 代计算平台
|性能级别|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|GP_Gen4_24
|:--- | --: |--: |--: |--: |--: |--: |
|硬件代次|4|4|4|4|4|4|
|vCore 数|1|2|4|8|16|24|
|内存 (GB)|7|14|28|56|112|168|
|列存储支持|是|是|是|是|是|是|
|内存中 OLTP 存储 (GB)|不适用|不适用|不适用|不适用|不适用|不适用|
|存储类型|高级（远程）存储|高级（远程）存储|高级（远程）存储|高级（远程）存储|高级（远程）存储|高级（远程）存储|
|IO 延迟（近似）|5-7 毫秒（写入）<br>5-10 毫秒（读取）|5-7 毫秒（写入）<br>5-10 毫秒（读取）|5-7 毫秒（写入）<br>5-10 毫秒（读取）|5-7 毫秒（写入）<br>5-10 毫秒（读取）|5-7 毫秒（写入）<br>5-10 毫秒（读取）|5-7 毫秒（写入）<br>5-10 毫秒（读取）|
|最大数据大小 (GB)|1024|1024|1536|3072|4096|4096|
|最大日志大小|307|307|461|922|1229|1229|
|TempDB 大小 (DB)|32|64|128|256|384|384|
|目标 IOPS (64 KB)|500|1000|2000|4000|7000|7000|
|最大并发工作线程数（请求数）|200|400|800|1600|3200|4800|
|允许的最大会话数|30000|30000|30000|30000|30000|30000|
|副本数|1|1|1|1|1|1|
|Multi-AZ|不适用|不适用|不适用|不适用|不适用|不适用|000
|读取横向扩展|不适用|不适用|不适用|不适用|不适用|不适用|
|随附的备份存储|1 倍数据库大小|1 倍数据库大小|1 倍数据库大小|1 倍数据库大小|1 倍数据库大小|1 倍数据库大小|
|||

### <a name="business-critical-service-tier"></a>“业务关键”服务层

#### <a name="generation-4-compute-platform"></a>第 4 代计算平台
|性能级别|BC_Gen4_1|BC_Gen4_2|BC_Gen4_4|BC_Gen4_8|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|硬件代次|4|4|4|4|4|4|
|vCore 数|1|2|4|8|16|24|
|内存 (GB)|7|14|28|56|112|168|
|列存储支持|是|是|是|是|是|是|
|内存中 OLTP 存储 (GB)|1|2|4|8|20 个|36|
|存储类型|本地 SSD|本地 SSD|本地 SSD|本地 SSD|本地 SSD|本地 SSD|
|最大数据大小 (GB)|1024|1024|1024|1024|1024|1024|
|最大日志大小|307|307|307|307|307|307|
|TempDB 大小 (DB)|32|64|128|256|384|384|
|目标 IOPS (64 KB)|5000|10000|20000|40000|80000|120000|
|IO 延迟（近似）|1-2 毫秒（写入）<br>1-2 毫秒（读取）|1-2 毫秒（写入）<br>1-2 毫秒（读取）|1-2 毫秒（写入）<br>1-2 毫秒（读取）|1-2 毫秒（写入）<br>1-2 毫秒（读取）|1-2 毫秒（写入）<br>1-2 毫秒（读取）|1-2 毫秒（写入）<br>1-2 毫秒（读取）|
|最大并发工作线程数（请求数）|200|400|800|1600|3200|4800|
|允许的最大会话数|30000|30000|30000|30000|30000|30000|
|副本数|3|3|3|3|3|3|
|Multi-AZ|不适用|不适用|不适用|不适用|不适用|不适用|
|读取横向扩展|是|是|是|是|是|是|
|随附的备份存储|1 倍数据库大小|1 倍数据库大小|1 倍数据库大小|1 倍数据库大小|1 倍数据库大小|1 倍数据库大小|
|||

## <a name="next-steps"></a>后续步骤

- 有关常见问题的解答，请参阅 [SQL 数据库常见问题解答](sql-database-faq.md)。
- 有关常规 Azure 限制的相关信息，请参阅 [Azure 订阅和服务限制、配额和约束](../azure-subscription-service-limits.md)。
