---
title: Azure Cosmos DB 查询语言中的 REPLICATE
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 REPLICATE。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 4b5abd0d2c081f901370ebc578047e3ccd05900c
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72914682"
---
# <a name="replicate-azure-cosmos-db"></a>REPLICATE (Azure Cosmos DB)
 将一个字符串值重复指定的次数。

## <a name="syntax"></a>语法

```sql
REPLICATE(<str_expr>, <num_expr>)
```  

## <a name="arguments"></a>参数

*str_expr*  
  是一个字符串表达式。

*num_expr*  
  是一个数值表达式。 如果 *num_expr* 为负或非有限，则结果未定义。

## <a name="return-types"></a>返回类型

  返回一个字符串表达式。

## <a name="remarks"></a>备注
  结果的最大长度为 10,000 个字符，即 (length(*str_expr*)  *  *num_expr*) <= 10,000。

## <a name="examples"></a>示例

  以下示例演示如何在查询中使用 `REPLICATE`。

```sql
SELECT REPLICATE("a", 3) AS replicate
```  

 下面是结果集。

```json
[{"replicate": "aaa"}]
```  

## <a name="next-steps"></a>后续步骤

- [字符串函数 Azure Cosmos DB](sql-query-string-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query replicate  -->
<!--New.date: 10/28/2019-->