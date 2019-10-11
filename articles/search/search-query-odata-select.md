---
title: OData select 参考 - Azure搜索
description: Azure 搜索查询中 select 语法的 OData 语言参考。
origin.date: 06/13/2019
ms.date: 09/26/2019
services: search
ms.service: search
ms.topic: conceptual
author: Brjohnstmsft
ms.author: v-tawe
manager: nitinme
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: 43ba8e25ab8d2e34e17f02576b53e9fc9bf26115
ms.sourcegitcommit: a5a43ed8b9ab870f30b94ab613663af5f24ae6e1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/30/2019
ms.locfileid: "71674505"
---
# <a name="odata-select-syntax-in-azure-search"></a>Azure 搜索中的 OData $select 语法

 可以使用 [OData **$select** 参数](query-odata-filter-orderby-syntax.md)选择要包含在 Azure 搜索的搜索结果中的字段。 本文详细介绍 **$select** 的语法。 有关如何在呈现搜索结果时使用 **$select** 的更多常规信息，请参阅[如何在 Azure 搜索中使用搜索结果](search-pagination-page-layout.md)。

## <a name="syntax"></a>语法

**$select** 参数确定在查询结果集中返回每个文档的哪些字段。 以下 EBNF（[扩展巴科斯-瑙尔范式](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)）定义 **$select** 参数的语法：

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
select_expression ::= '*' | field_path(',' field_path)*

field_path ::= identifier('/'identifier)*
```

交互式语法图也可用：

> [!div class="nextstepaction"]
> [Azure 搜索的 OData 语法图](https://azuresearch.github.io/odata-syntax-diagram/#select_expression)

> [!NOTE]
> 请参阅[适用于 Azure 搜索的 OData 表达式语法参考](search-query-odata-syntax-reference.md)以获取完整的 EBNF。

**$select** 参数有两种形式：

1. 单个星 (`*`)，指示应返回所有可检索字段，或
1. 以逗号分隔的字段路径列表，标识应返回的字段。

使用第二种形式时，只能在列表中指定可检索字段。

如果列出一个复杂字段而未显式指定其子字段，则所有可检索的子字段都将包含在查询结果集中。 例如，假设索引有一个 `Address` 字段，其中 `Street`、`City` 和 `Country` 子字段都是可检索的。 如果在 **$select** 中指定 `Address`，查询结果将包括所有三个子字段。

## <a name="examples"></a>示例

在结果中包括 `HotelId`、`HotelName` 和 `Rating` 顶级字段，以及 `Address` 的 `City` 子字段：

    $select=HotelId, HotelName, Rating, Address/City

示例结果可能如下所示：

```json
{
  "HotelId": "1",
  "HotelName": "Secret Point Motel",
  "Rating": 4,
  "Address": {
    "City": "New York"
  }
}
```

在结果中包括 `HotelName` 顶级字段，以及 `Address` 的所有子字段，以及 `Rooms` 集合中每个对象的 `Type` 和 `BaseRate` 子字段：

    $select=HotelName, Address, Rooms/Type, Rooms/BaseRate

示例结果可能如下所示：

```json
{
  "HotelName": "Secret Point Motel",
  "Rating": 4,
  "Address": {
    "StreetAddress": "677 5th Ave",
    "City": "New York",
    "StateProvince": "NY",
    "Country": "USA",
    "PostalCode": "10022"
  },
  "Rooms": [
    {
      "Type": "Budget Room",
      "BaseRate": 9.69
    },
    {
      "Type": "Budget Room",
      "BaseRate": 8.09
    }
  ]
}
```

## <a name="next-steps"></a>后续步骤  

- [如何在 Azure 搜索中使用搜索结果](search-pagination-page-layout.md)
- [Azure 搜索的 OData 表达式语言概述](query-odata-filter-orderby-syntax.md)
- [适用于 Azure 搜索的 OData 表达式语法参考](search-query-odata-syntax-reference.md)
- [搜索文档（Azure 搜索服务 REST API）](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
