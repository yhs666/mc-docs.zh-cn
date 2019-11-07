---
title: Azure Cosmos DB 查询语言中的 CONCAT
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 CONCAT。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: c43cda1119c0969c61f4c9ecf3d2997f9968be65
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72914751"
---
# <a name="concat-azure-cosmos-db"></a>CONCAT (Azure Cosmos DB)
 返回一个字符串，该字符串是连接两个或多个字符串值的结果。  

## <a name="syntax"></a>语法

```sql
CONCAT(<str_expr1>, <str_expr2> [, <str_exprN>])  
```  

## <a name="arguments"></a>参数

*str_expr*  
  是要连接到其他值的字符串表达式。 `CONCAT` 函数需要至少两个 *str_expr* 参数。  

## <a name="return-types"></a>返回类型

  返回一个字符串表达式。  

## <a name="examples"></a>示例

  以下示例返回将指定值串联后形成的字符串。  

```sql
SELECT CONCAT("abc", "def") AS concat  
```  

 下面是结果集。  

```json
[{"concat": "abcdef"}]  
```  

## <a name="next-steps"></a>后续步骤

- [字符串函数 Azure Cosmos DB](sql-query-string-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query concat  -->
<!--New.date: 10/28/2019-->