---
title: Azure Cosmos DB 查询语言中的 LOWER
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 LOWER。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 81a76f5c8d01d10e4fadce7c8935c689ad398d0f
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72914699"
---
# <a name="lower-azure-cosmos-db"></a>LOWER (Azure Cosmos DB)
 返回在将大写字符数据转换为小写后的字符串表达式。  

## <a name="syntax"></a>语法

```sql
LOWER(<str_expr>)  
```  

## <a name="arguments"></a>参数

*str_expr*  
  是一个字符串表达式。  

## <a name="return-types"></a>返回类型

  返回一个字符串表达式。  

## <a name="examples"></a>示例

  以下示例演示如何在查询中使用 `LOWER`。  

```sql
SELECT LOWER("Abc") AS lower
```  

 下面是结果集。  

```json
[{"lower": "abc"}]  

```  

## <a name="next-steps"></a>后续步骤

- [字符串函数 Azure Cosmos DB](sql-query-string-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query lower -->
<!--New.date: 10/28/2019-->