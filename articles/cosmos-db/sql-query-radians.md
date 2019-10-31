---
title: Azure Cosmos DB 查询语言中的 RADIANS
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 RADIANS。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 2aba5efc37b6c2e48f52dd8cd6c0517deb271878
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72914685"
---
# <a name="radians-azure-cosmos-db"></a>RADIANS (Azure Cosmos DB)
 返回输入的数值表达式（度）的弧度。  

## <a name="syntax"></a>语法

```sql
RADIANS (<numeric_expr>)  
```  

## <a name="arguments"></a>参数

*numeric_expr*  
  是一个数值表达式。  

## <a name="return-types"></a>返回类型

  返回一个数值表达式。  

## <a name="examples"></a>示例

  以下示例采用几个角度作为输入并返回其对应的弧度值。  

```sql
SELECT RADIANS(-45.01) AS r1, RADIANS(-181.01) AS r2, RADIANS(0) AS r3, RADIANS(0.1472738) AS r4, RADIANS(197.1099392) AS r5  
```  

  下面是结果集。  

```json
[{  
    "r1": -0.7855726963226477,  
    "r2": -3.1592204790349356,  
    "r3": 0,  
    "r4": 0.0025704127119236249,  
    "r5": 3.4402174274458375  
}]  
```  

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query radians -->
<!--New.date: 10/28/2019-->