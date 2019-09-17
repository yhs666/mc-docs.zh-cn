---
title: 简单实体类型 - LUIS
titleSuffix: Azure Cognitive Services
description: 简单实体是描述单个概念的泛型实体，通过机器学习的上下文习得。 由于简单实体通常是名称，例如公司名称、产品名称或其他类别的名称，因此，在使用简单实体时，请添加一个短语列表，以提升所用名称的信号。
services: cognitive-services
author: lingliw
manager: digimobile
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
origin.date: 07/24/2019
ms.date: 09/02/2019
ms.author: v-lingwu
ms.openlocfilehash: ab0083619acd6a9b41bf88d66bb1e6e1d802fa4b
ms.sourcegitcommit: 13642a99cc524a416b40635f48676bbf5cdcdf3d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2019
ms.locfileid: "70104201"
---
# <a name="simple-entity"></a>简单实体 

简单实体是描述单个概念的泛型实体，通过机器学习的上下文习得。 由于简单实体采用概括性的名称，例如公司名称、产品名称或其他类别的名称，因此，在使用简单实体时，请添加一个[短语列表](luis-concept-feature.md)，以提升所用名称的信号。 

**在以下情况下，非常适合使用此实体：**

* 数据格式不一致，但指示相同的事物。 

![简单实体](./media/luis-concept-entities/simple-entity.png)

## <a name="example-json"></a>示例 JSON

`Bob Jones wants 3 meatball pho`

在之前的陈述中，`Bob Jones` 被标记为一个简单的 `Customer` 实体。

从终结点返回的数据包括实体名称、从陈述中发现的文本、所发现文本的位置，以及评分：

```JSON
"entities": [
  {
  "entity": "bob jones",
  "type": "Customer",
  "startIndex": 0,
  "endIndex": 8,
  "score": 0.473899543
  }
]
```

|数据对象|实体名称|Value|
|--|--|--|
|简单实体|`Customer`|`bob jones`|

## <a name="next-steps"></a>后续步骤

在本[教程](luis-quickstart-primary-and-secondary-data.md)中，请使用**简单实体**从话语中提取雇佣工作名称的机器学习数据。 若要提高提取的准确性，请添加一个[短语列表](luis-concept-feature.md)，其中包含特定于简单实体的术语。