---
title: OData 集合运算符参考 - Azure 搜索
description: Azure 搜索查询中的 OData 集合运算符、any 和 all 以及 lambda 表达式。
origin.date: 06/13/2019
ms.date: 09/26/2019
services: search
ms.service: search
ms.topic: conceptual
author: brjohnstmsft
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
ms.openlocfilehash: 3af76a44398b45ab4ef57be10a3e4bc5ef7698e6
ms.sourcegitcommit: a5a43ed8b9ab870f30b94ab613663af5f24ae6e1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/30/2019
ms.locfileid: "71674510"
---
# <a name="odata-collection-operators-in-azure-search---any-and-all"></a>Azure 搜索中的 OData 集合运算符 - `any` 和 `all`

在编写 [OData 筛选器表达式](query-odata-filter-orderby-syntax.md)以用于 Azure 搜索时，对集合字段进行筛选通常很有用。 可以使用 `any` 和 `all` 运算符实现这一点。

## <a name="syntax"></a>语法

以下 EBNF（[扩展巴科斯-瑙尔范式](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)）定义了使用 `any` 或 `all` 的 OData 表达式的语法。

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
collection_filter_expression ::=
    field_path'/all(' lambda_expression ')'
    | field_path'/any(' lambda_expression ')'
    | field_path'/any()'

lambda_expression ::= identifier ':' boolean_expression
```

交互式语法图也可用：

> [!div class="nextstepaction"]
> [Azure 搜索的 OData 语法图](https://azuresearch.github.io/odata-syntax-diagram/#collection_filter_expression)

> [!NOTE]
> 请参阅[适用于 Azure 搜索的 OData 表达式语法参考](search-query-odata-syntax-reference.md)以获取完整的 EBNF。

有三种形式的表达式可以筛选集合。

- 前两个表达式遍历集合字段，将以 lambda 表达式形式给定的谓词应用于集合的每个元素。
  - 如果集合的每个元素的谓词都为 true，则使用 `all` 的表达式将返回 `true`。
  - 如果集合的至少一个元素的谓词为 true，则使用 `any` 的表达式将返回 `true`。
- 第三种形式的集合筛选器使用不带 lambda 表达式的 `any` 来测试集合字段是否为空。 如果集合有任何元素，则返回 `true`。 如果集合为空，则返回 `false`。

集合筛选器中的 **lambda 表达式**类似于编程语言中的循环体。 它定义了一个变量，称为**范围变量**，它在迭代期间保存集合的当前元素。 它还定义了另一个布尔表达式，该表达式是应用于集合的每个元素的范围变量的筛选条件。

## <a name="examples"></a>示例

匹配 `tags` 字段恰好包含字符串“wifi”的文档：

    tags/any(t: t eq 'wifi')

匹配其中 `ratings` 字段的每个元素介于 3 和 5 之间（包括 3 和 5）的文档：

    ratings/all(r: r ge 3 and r le 5)

匹配其中 `locations` 字段中任何地理坐标在给定多边形内的文档：

    locations/any(loc: geo.intersects(loc, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))

匹配其中 `rooms` 字段为空的文档：

    not rooms/any()

匹配对于所有房间 `rooms/amenities` 字段包含“tv”且 `rooms/baseRate` 小于 100 的文档：

    rooms/all(room: room/amenities/any(a: a eq 'tv') and room/baseRate lt 100.0)

## <a name="limitations"></a>限制

并非每个筛选器表达式功能都可在 Lambda 表达式主体中使用。 根据要筛选的集合字段的数据类型，限制会有所不同。 下表总结了这些限制。

[!INCLUDE [Limitations on OData lambda expressions in Azure Search](../../includes/search-query-odata-lambda-limitations.md)]

若要更详细地了解这些限制和相关示例，请参阅[排查 Azure 搜索中的集合筛选器问题](search-query-troubleshoot-collection-filters.md)。 若要更深入地了解为何存在这些限制，请参阅[了解 Azure 搜索中的集合筛选器](search-query-understand-collection-filters.md)。

## <a name="next-steps"></a>后续步骤  

- [Azure 搜索中的筛选器](search-filters.md)
- [Azure 搜索的 OData 表达式语言概述](query-odata-filter-orderby-syntax.md)
- [适用于 Azure 搜索的 OData 表达式语法参考](search-query-odata-syntax-reference.md)
- [搜索文档（Azure 搜索服务 REST API）](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
