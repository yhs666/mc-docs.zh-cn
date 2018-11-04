---
title: 检测特定于域的内容 - 计算机视觉
titleSuffix: Azure Cognitive Services
description: 使用计算机视觉 API 描述图像的相关概念。
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
origin.date: 08/29/2018
ms.date: 10/30/2018
ms.author: v-junlch
ms.openlocfilehash: a0850b94beb42366f77f5ea0cd683fbb25ec81c8
ms.sourcegitcommit: b8e99939a5493a15b78c32e87bfbf76a8c96a84a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "50409048"
---
# <a name="detecting-domain-specific-content"></a>检测特定领域的内容

除了标记和顶级分类以外，计算机视觉还支持专用（或特定于域的）信息。 可将专用信息作为独立方法来实现，也可将其与高级分类配合使用。 可以利用该信息，通过添加特定领域的模块来进一步优化“86 类”分类。

可以通过两个选项来使用特定领域的模型：

- 作用域分析  
  通过调用 HTTP POST 调用仅分析所选模型。 如果知道要使用的模型，请指定该模型的名称。 仅获取与该模型相关的信息。 例如，如果只进行名人识别，则可使用此选项。 响应包含一系列可能与之匹配的名人，以及置信度。
- 增强分析  
  通过分析，提供与“86 类”分类中的类别相关的更多详细信息。 如果应用程序用户除了获取一个或多个特定领域模型中的详细信息，还需要获取泛型图像分析信息，则可使用此选项。 调用此方法时，先调用“86 类”分类的分类器。 如果任何类别与已知或匹配模型的类别相匹配，将执行第二轮分类器调用。 例如，如果 HTTP POST 调用的 `details` 参数设置为“all”或包括“celebrities”，则在调用 86 类别分类器后，该方法将调用名人分类器。 如果图像被分类为 `people_` 或该类别的子类别，则调用名人分类器。

## <a name="listing-domain-specific-models"></a>列出特定于域的模型

可以列出计算机视觉支持的特定于域的模型。 目前，计算机视觉支持以下特定于域的模型，用于检测特定于域的内容：

| Name | 说明 |
|------|-------------|
| 名人 | 名人识别，支持属于 `people_` 类别的图像 |
| 地标 | 地标识别，支持属于 `outdoor_` 或 `building_` 类别的图像 |

### <a name="domain-model-list-example"></a>域模型列表示例

以下 JSON 响应列出了计算机视觉支持的特定于域的模型。

```json
{
    "models": [
        {
            "name": "celebrities",
            "categories": ["people_", "人_", "pessoas_", "gente_"]
        },
        {
            "name": "landmarks",
            "categories": ["outdoor_", "户外_", "屋外_", "aoarlivre_", "alairelibre_",
                "building_", "建筑_", "建物_", "edifício_"]
        }
    ]
}
```

## <a name="next-steps"></a>后续步骤

了解有关[对图像进行分类](concept-categorizing-images.md)的概念。

