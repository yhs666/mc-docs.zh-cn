---
title: 标记图像 - 计算机视觉
titleSuffix: Azure Cognitive Services
description: 使用计算机视觉 API 标记图像的相关概念。
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
origin.date: 08/29/2018
ms.date: 10/30/2018
ms.author: v-junlch
ms.openlocfilehash: ee1de3939123f5e4c08ea2669c145b817e93f427
ms.sourcegitcommit: b8e99939a5493a15b78c32e87bfbf76a8c96a84a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "50409108"
---
# <a name="tagging-images"></a>标记图像

计算机视觉在超过 2000 个可识别对象、生物、风景和操作的基础上返回标记。 当标记内容不明确或者不属常识时，API 响应会提供“提示”来澄清标记在已知场景中的含义。 标记不按分类来组织，且不存在继承层次结构。 内容标记集合在一起，形成图像“说明”的基础。该“说明”以人类可读语言显示，采用完整句子的格式。 请注意，图像说明目前只能使用英语。

上传图像或指定图像 URL 后，计算机视觉算法在对象、生物和图像中标识的操作的基础上输出标记。 标记不限于主体（例如前景中的人），还包括场景（户内或户外）、家具、工具、植物、动物、配件、小器具等。

## <a name="image-tagging-example"></a>图像标记示例

以下 JSON 响应表明计算机视觉在示例图像中检测到标记可视功能时所返回的内容。

![House_Yard](./Images/house_yard.png)上获取。

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

