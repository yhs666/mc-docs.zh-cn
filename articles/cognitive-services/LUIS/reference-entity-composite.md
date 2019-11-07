---
title: 复合实体类型 - LUIS
titleSuffix: Azure Cognitive Services
description: 复合实体由其他实体构成，例如预生成实体、简单实体、正则表达式实体和列表实体。 各种单独的实体构成整个实体。
services: cognitive-services
author: lingliw
manager: digimobile
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
origin.date: 07/24/2019
ms.date: 09/02/2019
ms.author: v-lingwu
ms.openlocfilehash: 3938293210824b81772363599131c8d132942b91
ms.sourcegitcommit: 13642a99cc524a416b40635f48676bbf5cdcdf3d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2019
ms.locfileid: "70104205"
---
# <a name="composite-entity"></a>复合实体 

复合实体由其他实体构成，例如预生成实体、简单实体、正则表达式实体和列表实体。 各种单独的实体构成整个实体。 

**如果数据具有以下特征，则非常适合使用此实体：**

* 彼此相关。 
* 在使用陈述的情况下彼此相关。
* 使用各种实体类型。
* 需要由客户端应用程序作为一个信息单元进行分组和处理。
* 包含需要机器学习的各种用户话语。

![复合实体](./media/luis-concept-entities/composite-entity.png)

## <a name="example-json"></a>示例 JSON

考虑一个预生成的 `number` 和 `Location::ToLocation` 的复合实体，它包含以下话语：

`book 2 tickets to paris`

注意数字 `2` 和 ToLocation `paris` 之间有单词，这些单词不属于任何实体。 [LUIS](luis-reference-regions.md) 网站中的已标记话语中使用的绿色下划线指示复合实体。

![复合实体](./media/luis-concept-data-extraction/composite-entity.png)

复合实体返回在 `compositeEntities` 数组中，且该复合中的所有实体也都返回在 `entities` 数组中：

```JSON

"entities": [
    {
    "entity": "2 tickets to cairo",
    "type": "ticketInfo",
    "startIndex": 0,
    "endIndex": 17,
    "score": 0.67200166
    },
    {
    "entity": "2",
    "type": "builtin.number",
    "startIndex": 0,
    "endIndex": 0,
    "resolution": {
        "subtype": "integer",
        "value": "2"
    }
    },
    {
    "entity": "cairo",
    "type": "builtin.geographyV2",
    "startIndex": 13,
    "endIndex": 17
    }
],
"compositeEntities": [
    {
    "parentType": "ticketInfo",
    "value": "2 tickets to cairo",
    "children": [
        {
        "type": "builtin.geographyV2",
        "value": "cairo"
        },
        {
        "type": "builtin.number",
        "value": "2"
        }
    ]
    }
]
```    

|数据对象|实体名称|Value|
|--|--|--|
|预构建实体 - 数量|"builtin.number"|"2"|
|预生成实体 - GeographyV2|"Location::ToLocation"|"paris"|

## <a name="next-steps"></a>后续步骤

在本[教程](luis-tutorial-composite-entity.md)中，添加**复合实体**来将提取的各种类型的数据捆绑到单个包含实体中。 通过捆绑数据，客户端应用程序可以轻松提取各种数据类型的相关数据。