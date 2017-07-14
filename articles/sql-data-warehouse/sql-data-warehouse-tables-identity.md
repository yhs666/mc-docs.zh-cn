---
title: "使用 IDENTITY 创建代理键 | Azure"
description: "了解如何使用 IDENTITY 在表上创建代理键。"
services: sql-data-warehouse
documentationcenter: NA
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: faa1034d-314c-4f9d-af81-f5a9aedf33e4
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
origin.date: 06/13/2017
ms.date: 07/17/2017
ms.author: v-yeche
ms.openlocfilehash: f03da6f5c2d70e2787d8199271734f67a5a731b8
ms.sourcegitcommit: 3727b139aef04c55efcccfa6a724978491b225a4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/05/2017
---
# 使用 IDENTITY 创建代理键
<a id="creating-surrogate-keys-using-identity" class="xliff"></a>
> [!div class="op_single_selector"]
> * [概述][Overview]
> * [数据类型][Data Types]
> * [分布][Distribute]
> * [索引][Index]
> * [分区][Partition]
> * [统计信息][Statistics]
> * [临时][Temporary]
> * [标识][Identity]
> 
> 

许多数据建模者在设计数据仓库模型时希望在其表上创建代理键。 可以使用 IDENTITY 属性简单有效地实现此目标，且不会影响加载性能。 

## 标识入门
<a id="getting-started-with-identity" class="xliff"></a>
首次创建表时，可以使用类似于以下语句的语法将表定义为具有 IDENTITY 属性：

```sql
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1) NOT NULL
,   C2 INT NULL
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;
```

然后，可以使用 `INSERT..SELECT` 来填充表。

## 行为
<a id="behavior" class="xliff"></a>
IDENTITY 属性用于在数据仓库中的所有分布区上进行扩展，不会影响加载性能。 因此，IDENTITY 的实现是朝着这些目标努力的。 本部分重点介绍了实现的微妙之处，以帮助你更全面地了解它们。  

### 值的分配
<a id="allocation-of-values" class="xliff"></a>
IDENTITY 属性不保证分配代理值时采用的顺序；这反映了 SQL Server 和 SQL DB 的行为。 但是，在 SQL 数据仓库中，无保证情况更加明显。 

请参考以下示例进行理解：

```sql
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1)    NOT NULL
,   C2 VARCHAR(30)              NULL
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;

INSERT INTO dbo.T1
VALUES (NULL);

INSERT INTO dbo.T1
VALUES (NULL);

SELECT *
FROM dbo.T1;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

请务必注意，分布区 1 中已经到达了两行。 第一行在 `C1` 列中具有代理值 1，第二行具有代理值 61。 这两个值都是由 IDENTITY 属性生成的。 但是，值的分配不是连续的。 此行为是设计使然。

### 倾斜的数据
<a id="skewed-data" class="xliff"></a> 
数据类型的值范围在各个分布区之间是均匀分配的。 如果某个分布式表存在数据倾斜，则可供该数据类型使用的值范围可能已过早地耗尽。 例如，如果所有数据都在单个分布区中用完，则该表实际上只能访问数据类型的值的 1/60。 出于此原因，IDENTITY 属性仅限用于 `INT` 和 `BIGINT` 数据类型。

### SELECT..INTO
<a id="selectinto" class="xliff"></a>
将某个现有标识列选入新表时，新列将继承 IDENTITY 属性，除非下列情况之一属实：
- SELECT 语句包含联接
- 使用 UNION 联接了多个 SELECT 语句
- 标识列在 select 列表中列出了多次
- 标识列是表达式的一部分

如果这些情况有任何一种属实，则列将创建为 NOT NULL 而非继承 IDENTITY 属性

### CREATE TABLE AS SELECT (CTAS)
<a id="create-table-as-select-ctas" class="xliff"></a>
CTAS 遵循针对 SELECT..INTO 记录的同一 SQL Server 行为。 但是，不能在语句本身的 `CREATE TABLE` 部分的列定义中指定 IDENTITY 属性。 也不能在 CTAS 的 `SELECT` 部分中使用 IDENTITY 函数。 若要填充表，需要使用 `CREATE TABLE` 来定义表，后跟 `INSERT..SELECT` 来填充该表。

## 显式将值插入到 IDENTITY 列
<a id="explicit-insertion-of-values-into-an-identity-column" class="xliff"></a> 
SQL 数据仓库支持 `SET IDENTITY_INSERT <your table> ON|OFF` 语法。 可以使用此语法显式将值插入到 IDENTITY 列中。

许多数据建模者希望为其维度中的某些行使用预定义的负值。 -1 或“未知成员”行是一个典型的示例。 

下一个脚本展示了如何使用 SET IDENTITY_INSERT 显式添加此行：

```sql
SET IDENTITY_INSERT dbo.T1 ON;

INSERT INTO dbo.T1
(   C1
,   C2
)
VALUES (-1,'UNKNOWN')
;

SET IDENTITY_INSERT dbo.T1 OFF;

SELECT  *
FROM    dbo.T1
;
```    

## 将数据加载到包含 IDENTITY 的表中
<a id="loading-data-into-a-table-with-identity" class="xliff"></a>

IDENTITY 属性的存在对数据加载代码有一些影响。 下一部分重点介绍了将数据加载到使用 IDENTITY 的表时可使用的一些基本模式。 

### 使用 PolyBase 加载数据
<a id="loading-with-polybase" class="xliff"></a>
若要将数据加载到表中并使用 IDENTITY 生成代理键，必须先创建表，然后使用 INSERT..SELECT 或 INSERT..VALUES 执行加载。

下面的示例重点介绍了基本模式：

```sql
--CREATE TABLE with IDENTITY
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1)
,   C2 VARCHAR(30)
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;

--Use INSERT..SELECT to populate the table from an external table
INSERT INTO dbo.T1
(C2)
SELECT  C2
FROM    ext.T1
;

SELECT  *
FROM    dbo.T1
;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

> [!NOTE] 
> 当前，将数据加载到包含 IDENTITY 列的表中时，无法使用 `CREATE TABLE AS SELECT`
> 

有关使用 bcp 加载数据的详细信息，请参阅以下文章：

- [使用 PolyBase 加载数据][]
- [PolyBase 最佳做法][]

### 使用 bcp 加载数据
<a id="loading-with-bcp" class="xliff"></a>
大容量复制程序 (bcp) 是一个命令行实用工具，可以用来将数据加载到 SQL 数据仓库中。 其参数之一 (-E) 控制当将数据加载到包含 IDENTITY 列的表时 bcp 的行为。 

当指定了 -E 时，将保留输入文件中为包含 IDENTITY 的列存储的值。 如果未指定 -E，则会忽略此列中的值。 如果未包括标识列，则会照常加载数据。 将根据属性的增量和种子策略来生成值。

有关使用 bcp 加载数据的详细信息，请参阅以下文章：

- [使用 bcp 加载数据][]
- [MSDN 中的 bcp][]

## 目录视图
<a id="catalog-views" class="xliff"></a>
SQL 数据仓库支持 `sys.identity_columns` 目录视图。 可以使用此视图将某个列标识为具有 IDENTITY 属性。

有关如何将 `sys.identity_columns` 与其他系统目录视图进行集成的示例可以帮助你更好地理解数据库架构。

```sql
SELECT  sm.name
,       tb.name
,       co.name
,       CASE WHEN ic.column_id IS NOT NULL
             THEN 1
        ELSE 0
        END AS is_identity 
FROM        sys.schemas AS sm
JOIN        sys.tables  AS tb           ON  sm.schema_id = tb.schema_id
JOIN        sys.columns AS co           ON  tb.object_id = co.object_id
LEFT JOIN   sys.identity_columns AS ic  ON  co.object_id = ic.object_id
                                        AND co.column_id = ic.column_id
WHERE   sm.name = 'dbo'
AND     tb.name = 'T1'
;
```

## 限制
<a id="limitations" class="xliff"></a>
IDENTITY 属性不能用于下列情况中：
- 列数据类型不是 INT 或 BIGINT
- 列同时是分发键
- 表是外部表 

SQL 数据仓库中不支持以下相关函数：

- [IDENTITY()][]
- [@@IDENTITY][]
- [SCOPE_IDENTITY][]
- [IDENT_CURRENT][]
- [IDENT_INCR][]
- [IDENT_SEED][]
- [DBCC CHECK_IDENT()][]

## 任务
<a id="tasks" class="xliff"></a>

本部分提供了在使用 IDENTITY 列时用于执行常见任务的一些示例代码。

> [!NOTE] 
> 在下列所有任务中，C1 列都是 IDENTITY。
> 

### 查找表的最高已分配值
<a id="find-the-highest-allocated-value-for-a-table" class="xliff"></a>
可以使用 `MAX()` 函数来确定为分布式表分配的最高值：

```sql
SELECT  MAX(C1)
FROM    dbo.T1
``` 

### 查找标识属性的种子和增量
<a id="find-the-seed-and-increment-for-the-identity-property" class="xliff"></a>
可以通过以下查询使用目录视图来查明表的标识增量和种子配置值： 

```sql
SELECT  sm.name
,       tb.name
,       co.name
,       ic.seed_value
,       ic.increment_value 
FROM        sys.schemas AS sm
JOIN        sys.tables  AS tb           ON  sm.schema_id = tb.schema_id
JOIN        sys.columns AS co           ON  tb.object_id = co.object_id
JOIN        sys.identity_columns AS ic  ON  co.object_id = ic.object_id
                                        AND co.column_id = ic.column_id
WHERE   sm.name = 'dbo'
AND     tb.name = 'T1'
;
```

## 后续步骤
<a id="next-steps" class="xliff"></a>

若要详细了解如何开发表，请参阅有关[表概述][Overview]、[表数据类型][Data Types]、[分布表][Distribute]、[为表编制索引][Index]、[对表进行分区][Partition]和[临时表][Temporary]的文章。  有关最佳实践的详细信息，请参阅 [SQL 数据仓库最佳实践][SQL Data Warehouse Best Practices]。  

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Identity]: ./sql-data-warehouse-tables-identity.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

[使用 bcp 加载数据]: /sql-data-warehouse/sql-data-warehouse-load-with-bcp/
[使用 PolyBase 加载数据]: /sql-data-warehouse/sql-data-warehouse-load-from-azure-blob-storage-with-polybase/
[PolyBase 最佳做法]: /sql-data-warehouse/sql-data-warehouse-load-polybase-guide/

<!--MSDN references-->
[Identity property]: https://msdn.microsoft.com/zh-cn/library/ms186775.aspx
[sys.identity_columns]: https://msdn.microsoft.com/zh-cn/library/ms187334.aspx
[IDENTITY()]: https://msdn.microsoft.com/zh-cn/library/ms189838.aspx
[@@IDENTITY]: https://msdn.microsoft.com/zh-cn/library/ms187342.aspx
[SCOPE_IDENTITY]: https://msdn.microsoft.com/zh-cn/library/ms190315.aspx
[IDENT_CURRENT]: https://msdn.microsoft.com/zh-cn/library/ms175098.aspx
[IDENT_INCR]: https://msdn.microsoft.com/zh-cn/library/ms189795.aspx
[IDENT_SEED]: https://msdn.microsoft.com/zh-cn/library/ms189834.aspx
[DBCC CHECK_IDENT()]: https://msdn.microsoft.com/zh-cn/library/ms176057.aspx

[MSDN 中的 bcp]: https://msdn.microsoft.com/zh-cn/library/ms162802.aspx

<!--Other Web references-->