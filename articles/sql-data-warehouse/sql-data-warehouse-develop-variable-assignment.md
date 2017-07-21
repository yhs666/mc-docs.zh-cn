---
title: "在 SQL 数据仓库中分配变量 | Azure"
description: "有关在开发解决方案时于 Azure SQL 数据仓库中分配 Transact-SQL 变量的技巧。"
services: sql-data-warehouse
documentationcenter: NA
author: rockboyfor
manager: jhubbard
editor: 
ms.assetid: 81ddc7cf-a6ba-4585-91a3-b6ea50f49227
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
origin.date: 10/31/2016
ms.date: 01/17/2017
ms.author: v-yeche
ms.openlocfilehash: 53492a18d15e8b1dcd1ac30d24bfb265c2c3653d
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# <a name="assign-variables-in-sql-data-warehouse"></a>在 SQL 数据仓库中分配变量
SQL 数据仓库中的变量是使用 `DECLARE` 语句或 `SET` 语句设置的。

以下各项是设置变量值的完全有效方式：

## <a name="setting-variables-with-declare"></a>使用 DECLARE 设置变量
使用 DECLARE 初始化变量是在 SQL 数据仓库中设置变量值的最灵活方式之一。

```sql
DECLARE @v  int = 0
;
```

你还可以使用 DECLARE 一次性设置多个变量。 不能使用 `SELECT` 或 `UPDATE` 执行此操作：

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

你无法在同一 DECLARE 语句中初始化和使用某个变量。 **无法**运行以下示例就说明了这一点，因为 @p1 在同一个 DECLARE 语句中初始化和使用。 这会导致错误。

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>使用 SET 设置值
SET 是设置单个变量的很常见方法。

以下所有示例都是使用 SET 设置变量的有效方式：

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

一次只能使用 SET 设置一个变量。 但是，如上所示，允许使用复合运算符。

## <a name="limitations"></a>限制
不能使用 SELECT 或 UPDATE 来分配变量。

## <a name="next-steps"></a>后续步骤
有关更多开发技巧，请参阅 [开发概述][development overview]。

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->