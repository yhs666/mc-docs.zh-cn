---
title: Azure Cosmos DB 查询语言中的 TAN
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 TAN。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 893ef014ff6c2e603d4c12ece4a5d5019924752d
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72914635"
---
# <a name="tan-azure-cosmos-db"></a>TAN (Azure Cosmos DB)
 返回在指定表达式中以弧度表示的指定角度的正切。  

## <a name="syntax"></a>语法

```sql
TAN (<numeric_expr>)  
```  

## <a name="arguments"></a>参数

*numeric_expr*  
  是一个数值表达式。  

## <a name="return-types"></a>返回类型

  返回一个数值表达式。  

## <a name="examples"></a>示例

  以下示例计算 PI()/2 的正切。 

```sql
SELECT TAN(PI()/2) AS tan 
```  

 下面是结果集。  

```json
[{"tan": 16331239353195370 }]  
```  

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query tan  -->
<!--New.date: 10/28/2019-->