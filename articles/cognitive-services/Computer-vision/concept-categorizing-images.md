---
title: 对图像进行分类 - 计算机视觉
titleSuffix: Azure Cognitive Services
description: 使用计算机视觉 API 对图像进行分类的相关概念。
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
origin.date: 08/29/2018
ms.date: 10/30/2018
ms.author: v-junlch
ms.openlocfilehash: 06e1cecdc21464a63daa57d2eed46e9006f7a3f7
ms.sourcegitcommit: b8e99939a5493a15b78c32e87bfbf76a8c96a84a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "50409046"
---
# <a name="categorizing-images"></a>对图像进行分类

除了标记和说明以外，计算机视觉还返回早期版本中定义的基于分类的类别。 这些类别按分类组织，存在可继承的父/子层次结构。 所有类别都是英文， 它们可单独使用或与我们的新标记模型结合使用。

## <a name="the-86-category-concept"></a>“86 类”概念

根据下图中所示的包含 86 个概念的列表，可以对图像按广泛到具体进行分类。 有关文本格式的完整分类，请参阅[类别分类](category-taxonomy.md)。

![分析类别](./Images/analyze_categories.png)

## <a name="image-categorization-examples"></a>图像分类示例

以下 JSON 响应表明计算机视觉在基于视觉特征对示例图像进行分类时返回的内容。

![屋顶的女人](./Images/woman_roof.png)

```json
{
    "categories": [
        {
            "name": "people_",
            "score": 0.81640625
        }
    ],
    "requestId": "bae7f76a-1cc7-4479-8d29-48a694974705",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

下表说明了典型的图像集以及计算机视觉为每个图像返回的类别。

| 映像 | 类别 |
|-------|----------|
| ![家庭照片](./Images/family_photo.png) | people_group |
| ![可爱的小狗](./Images/cute_dog.png) | animal_dog |
| ![户外山脉](./Images/mountain_vista.png) | outdoor_mountain |
| ![视觉分析食品面包](./Images/bread.png) | food_bread |

## <a name="next-steps"></a>后续步骤

了解[标记图像](concept-tagging-images.md)和[描述图像](concept-describing-images.md)的概念。

