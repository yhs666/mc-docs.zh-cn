---
title: Azure Cosmos DB 查询语言中的 TRUNC
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 TRUNC。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 09554499d006dee3a9f5baaacb2a92f471e1ebb2
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72914633"
---
# <a name="trunc-azure-cosmos-db"></a>TRUNC (Azure Cosmos DB)
 返回一个数值，截断到最接近的整数值。  

## <a name="syntax"></a>语法

```sql
TRUNC(<numeric_expr>)  
```  

## <a name="arguments"></a>参数

*numeric_expr*  
   是一个数值表达式。  

## <a name="return-types"></a>返回类型

  返回一个数值表达式。  

## <a name="examples"></a>示例

  以下示例将以下正数和负数截断到最接近的整数值。  

```sql
SELECT TRUNC(2.4) AS t1, TRUNC(2.6) AS t2, TRUNC(2.5) AS t3, TRUNC(-2.4) AS t4, TRUNC(-2.6) AS t5  
```  

 下面是结果集。  

```json
[{t1: 2, t2: 2, t3: 2, t4: -2, t5: -2}]  
```

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query trunc  -->
<!--New.date: 10/28/2019-->