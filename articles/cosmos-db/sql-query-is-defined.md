---
title: Azure Cosmos DB 查询语言中的 IS_DEFINED
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 IS_DEFINED。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 8424b5fcb59716c335dcba6b30cb1fc8469719fd
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72914722"
---
# <a name="is_defined-azure-cosmos-db"></a>IS_DEFINED (Azure Cosmos DB)
 返回一个布尔，它指示属性是否已经分配了值。  

## <a name="syntax"></a>语法

```sql
IS_DEFINED(<expr>)  
```  

## <a name="arguments"></a>参数

*expr*  
  是任何表达式。  

## <a name="return-types"></a>返回类型

  返回一个布尔表达式。  

## <a name="examples"></a>示例

  以下示例检查指定的 JSON 文档中是否存在某个属性。 第一个示例返回 true，因为“a” 存在；第二个示例返回 false，因为“b”不存在。  

```sql
SELECT IS_DEFINED({ "a" : 5 }.a) AS isDefined1, IS_DEFINED({ "a" : 5 }.b) AS isDefined2 
```  

 下面是结果集。  

```json
[{"isDefined1":true,"isDefined2":false}]  
```  

## <a name="next-steps"></a>后续步骤

- [类型检查函数 Azure Cosmos DB](sql-query-type-checking-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query is defined -->
<!--New.date: 10/28/2019-->