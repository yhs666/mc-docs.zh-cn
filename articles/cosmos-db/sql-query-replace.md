---
title: Azure Cosmos DB 查询语言中的 REPLACE
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 REPLACE。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: be454b54a7ef886dfae4a2e544596cf7a4130fb8
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72914681"
---
# <a name="replace-azure-cosmos-db"></a>REPLACE (Azure Cosmos DB)
 将出现的所有指定字符串值替换为另一个字符串值。  

## <a name="syntax"></a>语法

```sql
REPLACE(<str_expr1>, <str_expr2>, <str_expr3>)  
```  

## <a name="arguments"></a>参数

*str_expr1*  
  是要搜索的字符串表达式。  

*str_expr2*  
  要找到的字符串表达式。  

*str_expr3*  
  是用于替换 *str_expr1* 中的 *str_expr2* 匹配项的字符串表达式。  

## <a name="return-types"></a>返回类型

  返回一个字符串表达式。  

## <a name="examples"></a>示例

  以下示例演示如何在查询中使用 `REPLACE`。  

```sql
SELECT REPLACE("This is a Test", "Test", "desk") AS replace 
```  

 下面是结果集。  

```json
[{"replace": "This is a desk"}]  
```  

## <a name="next-steps"></a>后续步骤

- [字符串函数 Azure Cosmos DB](sql-query-string-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query replace -->
<!--New.date: 10/28/2019-->