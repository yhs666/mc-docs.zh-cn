---
title: Azure Cosmos DB 查询语言中的 FLOOR
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 FLOOR。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: b6644b9cdabf43bada26202b7423eac6f5ac6451
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72914737"
---
# <a name="floor-azure-cosmos-db"></a>FLOOR (Azure Cosmos DB)
 返回小于或等于指定数值表达式的最大整数。  

## <a name="syntax"></a>语法

```sql
FLOOR (<numeric_expr>)  
```  

## <a name="arguments"></a>参数

*numeric_expr*  
  是一个数值表达式。  

## <a name="return-types"></a>返回类型

  返回一个数值表达式。  

## <a name="examples"></a>示例

  以下示例显示了如何对正值、负值和零值使用 `FLOOR` 函数。  

```sql
SELECT FLOOR(123.45) AS fl1, FLOOR(-123.45) AS fl2, FLOOR(0.0) AS fl3  
```  

 下面是结果集。  

```json
[{fl1: 123, fl2: -124, fl3: 0}]  
```  

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query floor -->
<!--New.date: 10/28/2019-->