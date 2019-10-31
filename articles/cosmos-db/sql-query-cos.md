---
title: Azure Cosmos DB 查询语言中的 COS
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 COS。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: b8a33a0cb25066fbad3c94ab1958a4b33f422e52
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72914742"
---
# <a name="cos-azure-cosmos-db"></a>COS (Azure Cosmos DB)
 返回指定表达式中指定角度的三角余弦（弧度）。  

## <a name="syntax"></a>语法

```sql
COS(<numeric_expr>)  
```  

## <a name="arguments"></a>参数

*numeric_expr*  
  是一个数值表达式。  

## <a name="return-types"></a>返回类型

  返回一个数值表达式。  

## <a name="examples"></a>示例

  以下示例计算指定角度的 `COS`。  

```sql
SELECT COS(14.78) AS cos  
```  

 下面是结果集。  

```json
[{"cos": -0.59946542619465426}]  
```  

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query cos  -->
<!--New.date: 10/28/2019-->