---
title: Azure Cosmos DB 查询语言中的 UPPER
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 UPPER。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: f3f864efc912c7bd3d0ae3c66c7f5a77fb339873
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72914630"
---
# <a name="upper-azure-cosmos-db"></a>UPPER (Azure Cosmos DB)
 返回在将小写字符数据转换为大写后的字符串表达式。  

## <a name="syntax"></a>语法

```sql
UPPER(<str_expr>)  
```  

## <a name="arguments"></a>参数

*str_expr*  
  是一个字符串表达式。  

## <a name="return-types"></a>返回类型

  返回一个字符串表达式。  

## <a name="examples"></a>示例

  以下示例演示如何在查询中使用 `UPPER`  

```sql
SELECT UPPER("Abc") AS upper  
```  

 下面是结果集。  

```json
[{"upper": "ABC"}]  
```

## <a name="next-steps"></a>后续步骤

- [字符串函数 Azure Cosmos DB](sql-query-string-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query upper  -->
<!--New.date: 10/28/2019-->