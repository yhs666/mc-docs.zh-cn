---
title: Azure Cosmos DB 查询语言中的 ARRAY_LENGTH
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 ARRAY_LENGTH。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 889754782fef82f8d031053e1d2cc7f902161e75
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72914757"
---
# <a name="array_length-azure-cosmos-db"></a>ARRAY_LENGTH (Azure Cosmos DB)
 返回指定数组表达式的元素数。  

## <a name="syntax"></a>语法

```sql
ARRAY_LENGTH(<arr_expr>)  
```  

## <a name="arguments"></a>参数

*arr_expr*  
  是一个数组表达式。  

## <a name="return-types"></a>返回类型

  返回一个数值表达式。  

## <a name="examples"></a>示例

  以下示例演示了如何使用 `ARRAY_LENGTH` 获取数组的长度。  

```sql
SELECT ARRAY_LENGTH(["apples", "strawberries", "bananas"]) AS len  
```  

 下面是结果集。  

```json
[{"len": 3}]  
```  

## <a name="next-steps"></a>后续步骤

- [数组函数 Azure Cosmos DB](sql-query-array-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query array length  -->
<!--New.date: 10/28/2019-->