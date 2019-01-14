---
title: 将内容标记应用于图像 - 计算机视觉
titleSuffix: Azure Cognitive Services
description: 了解与计算机视觉 API 的图像标记功能相关的概念。
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
origin.date: 08/29/2018
ms.date: 01/08/2019
ms.author: v-junlch
ms.custom: seodec18
ms.openlocfilehash: 0fdb29b51196691960d47467d095d226ca8c599e
ms.sourcegitcommit: 90d5f59427ffa599e8ec005ef06e634e5e843d1e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2019
ms.locfileid: "54083573"
---
# <a name="applying-content-tags-to-images"></a>将内容标记应用于图像

计算机视觉在上千个可识别对象、生物、风景和操作的基础上返回标记。 当标记内容不明确或者不属常识时，API 响应会提供“提示”来澄清标记在已知场景中的含义。 标记不按分类来组织，且不存在继承层次结构。 内容标记集合在一起，形成图像“说明”的基础。该“说明”以人类可读语言显示，采用完整句子的格式。 请注意，图像说明目前只能使用英语。

上传图像或指定图像 URL 后，计算机视觉算法在对象、生物和图像中标识的操作的基础上输出标记。 标记不限于主体（例如前景中的人），还包括场景（户内或户外）、家具、工具、植物、动物、配件、小器具等。

## <a name="image-tagging-example"></a>图像标记示例

以下 JSON 响应表明计算机视觉在示例图像中检测到标记可视功能时所返回的内容。

![一座蓝色的房子和前院](./Images/house_yard.png)上获取。

```json
{
    "tags": [
        {
            "name": "grass",
            "confidence": 0.9999995231628418
        },
        {
            "name": "outdoor",
            "confidence": 0.99992108345031738
        },
        {
            "name": "house",
            "confidence": 0.99685388803482056
        },
        {
            "name": "sky",
            "confidence": 0.99532157182693481
        },
        {
            "name": "building",
            "confidence": 0.99436837434768677
        },
        {
            "name": "tree",
            "confidence": 0.98880356550216675
        },
        {
            "name": "lawn",
            "confidence": 0.788884699344635
        },
        {
            "name": "green",
            "confidence": 0.71250593662261963
        },
        {
            "name": "residential",
            "confidence": 0.70859086513519287
        },
        {
            "name": "grassy",
            "confidence": 0.46624681353569031
        }
    ],
    "requestId": "06f39352-e445-42dc-96fb-0a1288ad9cf1",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

## <a name="next-steps"></a>后续步骤

了解[对图像进行分类](concept-categorizing-images.md)和[描述图像](concept-describing-images.md)的概念。

<!-- Update_Description: wording update -->