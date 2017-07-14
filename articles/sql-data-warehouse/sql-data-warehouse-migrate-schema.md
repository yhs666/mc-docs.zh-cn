---
title: "将架构迁移到 SQL 数据仓库 | Azure"
description: "有关在开发解决方案时将架构迁移到 Azure SQL 数据仓库的技巧。"
services: sql-data-warehouse
documentationcenter: NA
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 538b60c9-a07f-49bf-9ea3-1082ed6699fb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
origin.date: 10/31/2016
ms.date: 07/17/2017
ms.author: v-yeche
ms.openlocfilehash: cc303fc1ec6490032d39455e4ec09c6c587caa36
ms.sourcegitcommit: 3727b139aef04c55efcccfa6a724978491b225a4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/05/2017
---
# 将架构迁移到 SQL 数据仓库
<a id="migrate-your-schema-to-sql-data-warehouse" class="xliff"></a>
以下摘要有助于理解 SQL Server 与 SQL 数据仓库之间的差异，方便用户迁移数据库。

## 表迁移
<a id="table-migration" class="xliff"></a>
在迁移表时，需熟悉 SQL 数据仓库表的表功能。  [表概述][table overview] 是一个不错的起点。  本文介绍了在创建表（例如表统计信息、分布、分区和索引）时需要考虑的最重要事项。  同时还介绍了一些 [不受支持的表功能][unsupported table features] 和解决方法。

SQL 数据仓库支持常见的业务数据类型。  请参阅[数据类型][data types]一文获取有关定义数据类型的指导。 

## 后续步骤
<a id="next-steps" class="xliff"></a>
成功将数据库架构迁移到 SQL 数据仓库后，请继续阅读下列文章之一：

* [迁移数据][Migrate your data]
* [迁移代码][Migrate your code]

有关 SQL 数据仓库最佳实践的详细信息，请参阅 [最佳实践][best practices] 一文。

<!--Image references-->

<!--Article references-->
[Migrate your code]: ./sql-data-warehouse-migrate-code.md
[Migrate your data]: ./sql-data-warehouse-migrate-data.md
[best practices]: ./sql-data-warehouse-best-practices.md
[table overview]: ./sql-data-warehouse-tables-overview.md
[unsupported table features]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[data types]: ./sql-data-warehouse-tables-data-types.md
[unsupported data types]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types

<!--MSDN references-->

<!--Other Web references-->