---
title: Azure Cosmos DB 查询语言中的 PI
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 PI。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: c9896b7ac061fcd83792857542afcbe1c4981617
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72914694"
---
# <a name="pi-azure-cosmos-db"></a>PI (Azure Cosmos DB)
 返回 PI 的常数值。  

## <a name="syntax"></a>语法

```sql
PI ()  
```  

## <a name="return-types"></a>返回类型

  返回一个数值表达式。  

## <a name="examples"></a>示例

  以下示例返回 `PI` 的值。  

```sql
SELECT PI() AS pi 
```  

 下面是结果集。  

```json
[{"pi": 3.1415926535897931}]  
```  

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query pi  -->
<!--New.date: 10/28/2019-->