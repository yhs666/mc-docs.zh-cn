---
title: SQL 数据仓库中的动态 SQL | Azure
description: 有关在开发解决方案时使用 Azure SQL 数据仓库中的动态 SQL 的技巧。
services: sql-data-warehouse
author: rockboyfor
manager: jhubbard
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
origin.date: 04/12/2018
ms.date: 04/24/2018
ms.author: v-yeche
ms.openlocfilehash: 8d4f52093c1d1552d3df4cd9b57151360b69b252
ms.sourcegitcommit: 0fedd16f5bb03a02811d6bbe58caa203155fd90e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2018
ms.locfileid: "32121151"
---
# <a name="dynamic-sql-in-sql-data-warehouse"></a>SQL 数据仓库中的动态 SQL
有关在开发解决方案时使用 Azure SQL 数据仓库中的动态 SQL 的技巧。

## <a name="dynamic-sql-example"></a>动态 SQL 示例

为 SQL 数据仓库开发应用程序代码时，可能需要使用动态 SQL 来帮助提供灵活、通用且模块化的解决方案。 SQL 数据仓库目前不支持 Blob 数据类型。 不支持 blob 数据类型可能会限制字符串的大小，因为 blob 数据类型包括 varchar(max) 和 nvarchar(max) 类型。 如果已在应用程序代码中使用这些类型构建大型字符串，则需要将代码分解成块，并改用 EXEC 语句。

一个简单的示例：

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

如果字符串较短，则可以像平时一样使用 [sp_executesql](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-executesql-transact-sql)。

> [!NOTE]
> 作为动态 SQL 执行的语句仍将遵循所有 TSQL 验证规则。
> 
> 

## <a name="next-steps"></a>后续步骤
有关更多开发技巧，请参阅[开发概述](sql-data-warehouse-overview-develop.md)。

