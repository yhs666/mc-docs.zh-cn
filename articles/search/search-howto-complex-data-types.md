---
title: 如何为复杂数据类型建模 - Azure 搜索
description: 可以在 Azure 搜索索引中使用 ComplexType 和 Collections 数据类型为嵌套或分层数据结构建模。
author: brjohnstmsft
manager: jlembicz
ms.author: v-biyu
tags: complex data types; compound data types; aggregate data types
services: search
ms.service: search
ms.topic: conceptual
origin.date: 05/13/2019
ms.date: 06/03/2019
ms.custom: seodec2018
ms.openlocfilehash: 8780ca103bb5538492fd9cdfb95cef623e7f6ebe
ms.sourcegitcommit: bf4afcef846cc82005f06e6dfe8dd3b00f9d49f3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2019
ms.locfileid: "66004737"
---
# <a name="how-to-model-complex-data-types-in-azure-search"></a>如何在 Azure 搜索中为复杂数据类型建模

用于填充 Azure 搜索索引的外部数据集有时包含分层或嵌套的子结构。 示例可能包括单个客户的多个位置和电话号码、单个 SKU 的多个颜色和大小、一本书籍的多位作者等等。 在建模术语中，这些结构可能称为“复杂数据类型”、“组合数据类型”、“复合数据类型”或“聚合数据类型”。     在 Azure 搜索术语中，复杂类型是包含子级（子字段）的字段，而这些子级本身可能是简单或复杂的。 这类似于编程语言中的结构化数据类型。 复杂字段可以是代表文档中单个对象的单个字段，也可以是代表对象数组的集合

Azure 搜索原生支持复杂类型和集合。 结合使用这些类型几乎可为 Azure 搜索索引中的任何嵌套式 JSON 结构建模。 在旧版的 Azure 搜索 API 中，只能导入平展的行集。 在最新版本中，索引可以更密切地对应于源数据。 换言之，如果源数据使用复杂类型，则索引也可以使用复杂类型。

若要开始，我们建议使用 [Hotels 数据集](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/README.md)，可以在 Azure 门户上“导入数据”向导中加载该数据集。  该向导会检测源中的复杂类型，并根据检测到的结构建议一个索引架构。

> [!Note]
> `api-version=2019-05-06` 正式支持复杂类型。 
>
> 如果你的搜索解决方案是基于以前的解决方法（集合中的平展数据集）生成的，应更改索引，使之包含最新 API 版本支持的复杂类型。 有关升级的 API 版本的详细信息，请参阅[升级到最新的 REST API 版本](search-api-migration.md)或[升级到最新的 .NET SDK 版本](search-dotnet-sdk-migration-version-9.md)。

## <a name="example-of-a-complex-structure"></a>复杂结构的示例

以下 JSON 文档由简单字段和复杂字段构成。 复杂字段（例如 `Address` 和 `Rooms`）包含子字段。 `Address` 包含这些子字段的单个值集，因为它是文档中的单一对象。 相反，`Rooms` 包含其子字段的多个值集，集合中的每个对象各有一个值集。

```json
{
  "HotelId": "1",
  "HotelName": "Secret Point Motel",
  "Description": "Ideally located on the main commercial artery of the city in the heart of New York.",
  "Address": {
    "StreetAddress": "677 5th Ave",
    "City": "New York",
    "StateProvince": "NY"
  },
  "Rooms": [
    {
      "Description": "Budget Room, 1 Queen Bed (Cityside)",
      "Type": "Budget Room",
      "BaseRate": 96.99,
    },
    {
      "Description": "Deluxe Room, 2 Double Beds (City View)",
      "Type": "Deluxe Room",
      "BaseRate": 150.99,
    },
  ]
}
```

## <a name="creating-complex-fields"></a>创建复杂字段

与处理任何索引定义时一样，可以使用门户、[REST API](https://docs.microsoft.com/rest/api/searchservice/create-index) 或 [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index?view=azure-dotnet) 创建包含复杂类型的架构。 

以下示例演示了包含简单字段、集合与复杂类型的 JSON 索引架构。 请注意，在复杂类型中，与顶级字段一样，每个子字段包含一个类型，有时还包含属性。 架构对应于以上示例数据。 `Address` 是一个非集合的复杂字段（一家酒店只有一个地址）。 `Rooms` 是复杂集合字段（一家酒店有多间客房）。

<!---
For indexes used in a [push-model data import](search-what-is-data-import.md) strategy, where you are pushing a JSON data set to an Azure Search index, you can only have the basic syntax shown here: single complex types like `Address`, or a `Collection(Edm.ComplexType)` like `Rooms`. You cannot have complex types nested inside other complex types in an index used for push-model data ingestion.

Indexers are a different story. When defining an indexer, in particular one used to build a knowledge store, your index can have nested complex types. An indexer is able to hold a chain of complex data structures in-memory, and when it includes a skillset, it can support highly complex data forms. For more information and an example, see [How to get started with knowledge store](knowledge-store-howto.md).
-->

```json
{
  "name": "hotels",
  "fields": [
    { "name": "HotelId", "type": "Edm.String", "key": true, "filterable": true  },
    { "name": "HotelName", "type": "Edm.String", "searchable": true, "filterable": false },
    { "name": "Description", "type": "Edm.String", "searchable": true, "analyzer": "en.lucene" },
    { "name": "Address", "type": "Edm.ComplexType",
      "fields": [
        { "name": "StreetAddress", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "searchable": true },
        { "name": "City", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
        { "name": "StateProvince", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true }
      ]
    },
    { "name": "Rooms", "type": "Collection(Edm.ComplexType)",
      "fields": [
        { "name": "Description", "type": "Edm.String", "searchable": true, "analyzer": "en.lucene" },
        { "name": "Type", "type": "Edm.String", "searchable": true },
        { "name": "BaseRate", "type": "Edm.Double", "filterable": true, "facetable": true }
      ]
    }
  ]
}
```

## <a name="updating-complex-fields"></a>更新复杂字段

一般情况下，应用于字段的所有[重建索引规则](search-howto-reindex.md)仍会应用于复杂字段。 在此处重述一些主要规则以及添加字段并不需要重建索引，但大多数修改操作需要重建索引。

### <a name="structural-updates-to-the-definition"></a>对定义的结构更新

随时可以将新的子字段添加到复杂字段，而无需重建索引。 例如，允许将“ZipCode”添加到 `Address`或者将“Amenities”添加到 `Rooms`，就如同将顶级字段添加到索引一样。 在通过更新数据显式填充新字段之前，现有文档将对这些字段使用 null 值。

请注意，在复杂类型中，与顶级字段一样，每个子字段包含一个类型，有时还包含属性

### <a name="data-updates"></a>数据更新

对于复杂字段和简单字段而言，使用 upload 操作更新索引中现有文档的过程是相同的 -- 将替换所有字段。 但是，merge（应用于现有文档时使用 mergeOrUpload）对所有字段的运行方式不同。 具体而言，merge 无法合并集合中的元素。 对于基元类型的集合以及复杂集合，都是如此。 若要更新集合，需要检索整个集合值，进行更改，然后在索引 API 请求中包含新的集合。


## <a name="searching-complex-fields"></a>搜索复杂字段

可按预期方式对复杂类型运行自由形式的搜索表达式。 如果文档中的任何位置存在任何一个匹配的可搜索字段或子字段，则该文档本身就是一个匹配项。 

如果使用多个字词或运算符，并且某些字词指定了字段名（可以使用 [Lucene 语法](query-lucene-syntax.md)来指定），则查询会变得更微妙。 例如，此查询尝试将两个字词“Portland”和“OR”与 Address 字段的两个子字段相匹配：

```json
search=Address/City:Portland AND Address/State:OR
```

此类查询与全文搜索不相关（这与筛选器不同。在筛选器中，可以使用 any 或 all 来关联基于复杂集合的子字段运行的查询，如同 SQL 中的关联子查询一样）。 这意味着，上述 Lucene 查询将返回包含“Portland, Maine”和“Portland, Oregon”以及俄勒冈州 (Oregon) 其他城市的文档。 这是因为，将会针对整个文档中指定字段的所有值评估每个子句，因此不存在“当前子文档”的概念。 

 

## <a name="selecting-complex-fields"></a>选择复杂字段

`$select` 参数用于选择要在搜索结果中返回哪些字段。 若要使用此参数来选择复杂字段的特定子字段，请包含斜杠 (`/`) 分隔的父字段和子字段。

```json
$select=HotelName, Address/City, Rooms/BaseRate
```

如果希望这些字段在搜索结果中出现，必须在索引中将其标记为可检索。 只有标记为可检索的字段才能在 `$select` 语句中使用。 


## <a name="filter-facet-and-sort-complex-fields"></a>筛选、分面和排序复杂字段

用作筛选和带字段搜索的 [OData 路径语法](query-odata-filter-orderby-syntax.md)同样也可用于分面、排序和选择搜索请求中的字段。 对于复杂类型，可以应用规则来控制可将哪些子字段标记为可排序或可分面。 

### <a name="faceting-sub-fields"></a>分面子字段 

除非类型为 `Edm.GeographyPoint` 或 `Collection(Edm.GeographyPoint)`，否则任何子字段都可标记为可分面。 

返回分面导航结构的文档计数时，计数相对于父文档（酒店），而不是相对于复杂集合中的嵌套文档（客房）。 例如，假设某家酒店有 20 间“套房”类型的客房。 在指定分面参数 `facet=Rooms/Type` 的情况下，分面计数将是父文档（酒店）的计数，而不是中间子文档（客房）的计数。 

### <a name="sorting-complex-fields"></a>排序复杂字段

排序操作将应用于文档（酒店）而不是子文档（客房）。 使用复杂类型集合（例如客房）时必须认识到，根据无法按“客房”排序。 事实上，无法按任何集合进行排序。 

如果字段是单值的（不管是简单字段，还是复杂类型中的子字段），排序操作可正常运行。 例如，`$orderby=Address/ZipCode` 复杂类型是可排序的，因为每家酒店只有一个邮政编码。 

在索引字段中重述有关排序的规则的子句必须标记为可筛选且可排序，这样才能在 `$orderby` 语句中使用。 

## <a name="next-steps"></a>后续步骤

 尝试在“导入数据”向导中练习 [Hotels 数据集](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/README.md)。  需要使用自述文件中提供的 Cosmos DB 连接信息来访问这些数据。 
 
 获取该信息后，向导中的第一步是创建新的 Azure Cosmos DB 数据源。 然后，在向导中进入目标索引页后，会看到使用复杂类型的索引。 请创建并加载此索引，然后执行查询来了解新结构。

> [!div class="nextstepaction"]
> [快速入门：用于导入、索引编制和查询的门户向导](search-get-started-portal.md)