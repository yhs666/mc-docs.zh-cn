---
title: Azure Cosmos DB 查询语言中的 COT
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 COT。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 99727ece1f7b47b1f399f89720c698a7c99cba11
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72914740"
---
# <a name="cot-azure-cosmos-db"></a>COT (Azure Cosmos DB)
 返回指定数值表达式中指定角度的三角余切。  

## <a name="syntax"></a>语法

```sql
COT(<numeric_expr>)  
```  

## <a name="arguments"></a>参数

*numeric_expr*  
  是一个数值表达式。  

## <a name="return-types"></a>返回类型

  返回一个数值表达式。  

## <a name="examples"></a>示例

  以下示例计算指定角度的 `COT`。  

```sql
SELECT COT(124.1332) AS cot  
```  

 下面是结果集。  

```json
[{"cot": -0.040311998371148884}]  
```  

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query cot  -->
<!--New.date: 10/28/2019-->