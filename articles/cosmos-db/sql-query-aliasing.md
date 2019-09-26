---
title: 在 Azure Cosmos DB 中指定别名
description: 了解如何为 Azure Cosmos DB SQL 查询中的值指定别名
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 06/20/2019
ms.date: 09/30/2019
ms.author: v-yeche
ms.openlocfilehash: 0ca5860c9f43e3e08579af286d9362d689597eb6
ms.sourcegitcommit: 0d07175c0b83219a3dbae4d413f8e012b6e604ed
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/26/2019
ms.locfileid: "71306769"
---
# <a name="aliasing-in-azure-cosmos-db"></a>在 Azure Cosmos DB 中指定别名

可以显式为查询中的值指定别名。 如果查询包含两个同名的属性，请使用别名来重命名其中一个或两个属性，以便可以在投影的结果中消除其歧义。

## <a name="examples"></a>示例

如以下示例所示，将第二个值投影为 `NameInfo` 时，用于别名的 AS 关键字是可选的：

```sql
    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo,
           { "name": f.id } NameInfo
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

其结果是：

```json
    [{
      "AddressInfo": {
        "state": "WA",
        "city": "Seattle"
      },
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]
```

## <a name="next-steps"></a>后续步骤

- [Azure Cosmos DB .NET 示例](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [SELECT 子句](sql-query-select.md)
- [FROM 子句](sql-query-from.md)

<!-- Update_Description: wording update, update link -->