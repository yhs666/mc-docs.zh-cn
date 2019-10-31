---
title: Azure Cosmos DB 查询语言中的 EXP
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 EXP。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 2ef2a3deed621b0d13fc27108acab3af0a583dff
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72914734"
---
# <a name="exp-azure-cosmos-db"></a>EXP (Azure Cosmos DB)
 返回指定数值表达式的指数值。  

## <a name="syntax"></a>语法

```sql
EXP (<numeric_expr>)  
```  

## <a name="arguments"></a>参数

*numeric_expr*  
  是一个数值表达式。  

## <a name="return-types"></a>返回类型

  返回一个数值表达式。  

## <a name="remarks"></a>备注

  常量 **e** (2.718281…) 是自然对数的底。  

  某个数字的指数是对常量 **e** 执行该次数的乘幂计算。 例如 EXP(1.0) = e^1.0 = 2.71828182845905，EXP(10) = e^10 = 22026.4657948067。 

  某个数的自然对数的指数就是该数本身：EXP (LOG (n)) = n。 并且某个数的指数的自然对数也是该数本身：LOG (EXP (n)) = n。  

## <a name="examples"></a>示例

  以下示例声明一个变量并返回指定变量 (10) 的指数值。  

```sql
SELECT EXP(10) AS exp  
```  

 下面是结果集。  

```json
[{exp: 22026.465794806718}]  
```  

 以下示例返回 20 的自然对数的指数值和 20 的指数的自然对数。 因为这些函数是另一个的反函数，因此，在两个示例中，返回值在进行浮点算术舍入后都是 20。 

```sql
SELECT EXP(LOG(20)) AS exp1, LOG(EXP(20)) AS exp2  
```  

 下面是结果集。  

```json
[{exp1: 19.999999999999996, exp2: 20}]  
```  

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query exp -->
<!--New.date: 10/28/2019-->