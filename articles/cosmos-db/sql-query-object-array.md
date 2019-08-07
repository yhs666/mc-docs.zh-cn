---
title: 在 Azure Cosmos DB 中使用数组和对象
description: 了解 Azure Cosmos DB 的数组和对象创建 SQL 语法。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 06/21/2019
ms.date: 07/29/2019
ms.author: v-yeche
ms.openlocfilehash: 7214a9da0fd38dd4ec54d72c5e41173f10b3cc1a
ms.sourcegitcommit: 5a4a826eea3914911fd93592e0f835efc9173133
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/31/2019
ms.locfileid: "68672283"
---
# <a name="working-with-arrays-and-objects-in-azure-cosmos-db"></a>在 Azure Cosmos DB 中使用数组和对象

Azure Cosmos DB SQL API 的一个重要功能是创建数组和对象。

## <a name="arrays"></a>数组

可以构造数组，如以下示例所示：

```sql
    SELECT [f.address.city, f.address.state] AS CityState
    FROM Families f
```

其结果是：

```json
    [
      {
        "CityState": [
          "Seattle",
          "WA"
        ]
      },
      {
        "CityState": [
          "NY", 
          "NY"
        ]
      }
    ]
```

还可以使用 [ARRAY 表达式](sql-query-subquery.md#array-expression)根据[子查询](sql-query-subquery.md)的结果构造数组。 此查询获取数组中子项的所有不同给定名称。

```sql
SELECT f.id, ARRAY(SELECT DISTINCT VALUE c.givenName FROM c IN f.children) as ChildNames
FROM f
```

<a name="Iteration"></a>
## <a name="iteration"></a>迭代

SQL API 支持循环访问 JSON 数组，它可以通过 FROM 源中的 [IN 关键字](sql-query-keywords.md#in)添加一个新的构造。 在以下示例中：

```sql
    SELECT *
    FROM Families.children
```

其结果是：

```json
    [
      [
        {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy"}]
        }
      ], 
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        }, 
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]
```

下一个查询循环访问 `Families` 容器中的 `children`。 输出的数组与前面的查询不同。 此示例拆分 `children` 并将结果平展为单个数组：  

```sql
    SELECT *
    FROM c IN Families.children
```

其结果是：

```json
    [
      {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy" }]
      },
      {
          "familyName": "Merriam",
          "givenName": "Jesse",
          "gender": "female",
          "grade": 1
      },
      {
          "familyName": "Miller",
          "givenName": "Lisa",
          "gender": "female",
          "grade": 8
      }
    ]
```

可以进一步筛选该数组的每个条目，如以下示例所示：

```sql
    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8
```

其结果是：

```json
    [{
      "givenName": "Lisa"
    }]
```

还可基于数组迭代的结果进行聚合。 例如，以下查询计数所有家庭中的孩子数目。

```sql
    SELECT COUNT(child)
    FROM child IN Families.children
```

其结果是：

```json
    [
      {
        "$1": 3
      }
    ]
```

## <a name="next-steps"></a>后续步骤

- [入门](sql-query-getting-started.md)
- [Azure Cosmos DB .NET 示例](https://github.com/Azure/azure-cosmosdb-dotnet)
- [联接](sql-query-join.md)

<!-- Update_Description: wording update, update link -->