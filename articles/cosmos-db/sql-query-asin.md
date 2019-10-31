---
title: Azure Cosmos DB 查询语言中的 ASIN
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 ASIN。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: cc20040dec74447c8d5d61cdc5c8708848031748
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72914756"
---
# <a name="asin-azure-cosmos-db"></a>ASIN (Azure Cosmos DB)
 返回角度（弧度），其正弦是指定的数值表达式。 也被称为反正弦。  

## <a name="syntax"></a>语法

```sql
ASIN(<numeric_expr>)  
```  

## <a name="arguments"></a>参数

*numeric_expr*  
  是一个数值表达式。  

## <a name="return-types"></a>返回类型

  返回一个数值表达式。  

## <a name="examples"></a>示例

  以下示例返回 -1 的 `ASIN`。 

```sql
SELECT ASIN(-1) AS asin  
```  

 下面是结果集。  

```json
[{"asin": -1.5707963267948966}]  
```  

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query asin  -->
<!--New.date: 10/28/2019-->