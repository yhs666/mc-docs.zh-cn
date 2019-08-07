---
title: Azure Cosmos DB 中的 OFFSET LIMIT 子句
description: 了解 Azure Cosmos DB 的 OFFSET LIMIT 子句。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 06/10/2019
ms.date: 07/29/2019
ms.author: v-yeche
ms.openlocfilehash: c8850f28e8ad02bdfa8c16e2e1d56fd746a8460d
ms.sourcegitcommit: 5a4a826eea3914911fd93592e0f835efc9173133
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/31/2019
ms.locfileid: "68672285"
---
# <a name="offset-limit-clause"></a>OFFSET LIMIT 子句

OFFSET LIMIT 子句是一个可选子句，可以跳过它，然后从查询中获取一些值。 必须在 OFFSET LIMIT 子句中指定 OFFSET 计数和 LIMIT 计数。

将 OFFSET LIMIT 与 ORDER BY 子句结合使用时，将通过跳过然后提取排序值来生成结果集。 如果不使用 ORDER BY 子句，则会生成值的确定顺序。

## <a name="syntax"></a>语法

```sql  
OFFSET <offset_amount> LIMIT <limit_amount>
```  

## <a name="arguments"></a>参数

- `<offset_amount>`

    指定查询结果应跳过的项数（整数）。

- `<limit_amount>`

    指定查询结果应包含的项数（整数）

## <a name="remarks"></a>备注

必须在 OFFSET LIMIT 子句中同时指定 OFFSET 计数和 LIMIT 计数。 如果使用可选的 `ORDER BY` 子句，将会通过跳过排序值来生成结果集。 否则，查询将返回固定顺序的值。 目前，只有单个分区中的查询支持此子句，跨分区查询尚不支持它。

## <a name="examples"></a>示例

例如，以下查询跳过第一个值并返回第二个值（按居住城市名称的顺序）：

```sql
    SELECT f.id, f.address.city
    FROM Families f
    ORDER BY f.address.city
    OFFSET 1 LIMIT 1
```

其结果是：

```json
    [
      {
        "id": "AndersenFamily",
        "city": "Seattle"
      }
    ]
```

以下查询跳过第一个值并返回第二个值（不排序）：

```sql
   SELECT f.id, f.address.city
    FROM Families f
    OFFSET 1 LIMIT 1
```

其结果是：

```json
    [
      {
        "id": "WakefieldFamily",
        "city": "Seattle"
      }
    ]
```

## <a name="next-steps"></a>后续步骤

- [入门](sql-query-getting-started.md)
- [SELECT 子句](sql-query-select.md)
- [ORDER BY 子句](sql-query-order-by.md)

<!-- Update_Description: wording update, update link -->