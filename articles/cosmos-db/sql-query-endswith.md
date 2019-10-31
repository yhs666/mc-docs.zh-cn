---
title: Azure Cosmos DB 查询语言中的 ENDSWITH
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 ENDSWITH。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: fae513454a1d7899da302a987d4809b68a289d17
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72914736"
---
# <a name="endswith-azure-cosmos-db"></a>ENDSWITH (Azure Cosmos DB)
 返回一个布尔值，指示第一个字符串表达式是否以第二个字符串表达式结尾。  

## <a name="syntax"></a>语法

```sql
ENDSWITH(<str_expr1>, <str_expr2>)  
```  

## <a name="arguments"></a>参数

*str_expr1*  
  是一个字符串表达式。  

*str_expr2*  
  是要与 *str_expr1* 的结尾进行比较的字符串表达式。  

## <a name="return-types"></a>返回类型

  返回一个布尔表达式。  

## <a name="examples"></a>示例

  以下示例的返回结果指示“abc”是否以“b”和“bc”结尾。  

```sql
SELECT ENDSWITH("abc", "b") AS e1, ENDSWITH("abc", "bc") AS e2 
```  

 下面是结果集。  

```json
[{"e1": false, "e2": true}]  
```  

## <a name="next-steps"></a>后续步骤

- [字符串函数 Azure Cosmos DB](sql-query-string-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query endswith  -->
<!--New.date: 10/28/2019-->