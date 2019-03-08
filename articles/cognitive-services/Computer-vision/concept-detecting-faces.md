---
title: 检测人脸 - 计算机视觉
titleSuffix: Azure Cognitive Services
description: 与计算机视觉 API 的人脸检测功能相关的概念。
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
origin.date: 02/11/2019
ms.date: 02/27/2019
ms.author: v-junlch
ms.custom: seodec18
ms.openlocfilehash: 06b8761c5300a886ce42a27d5b5b940f06b029a3
ms.sourcegitcommit: ea33f8dbf7f9e6ac90d328dcd8fb796241f23ff7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/01/2019
ms.locfileid: "57204164"
---
# <a name="face-detection-with-computer-vision"></a>使用计算机视觉进行人脸检测

计算机视觉可检测图像中的人脸，并为每个检测到的人脸生成年龄、性别和人脸边框。 

> [!NOTE]
> Azure [人脸](/cognitive-services/face/)服务也提供此功能。 有关人脸分析（包括人脸识别和姿势检测）的详细信息，请参阅此替代方案。 

## <a name="face-detection-examples"></a>人脸检测示例

以下示例演示了计算机视觉为包含单个人脸的图像返回的 JSON 响应。

![视觉分析屋顶的女人人脸](./Images/woman_roof_face.png)

```json
{
    "faces": [
        {
            "age": 23,
            "gender": "Female",
            "faceRectangle": {
                "top": 45,
                "left": 194,
                "width": 44,
                "height": 44
            }
        }
    ],
    "requestId": "8439ba87-de65-441b-a0f1-c85913157ecd",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Png"
    }
}
```

第二个示例演示了为包含多个人脸的图像返回的 JSON 响应。

![视觉分析家庭照片中的人脸](./Images/family_photo_face.png)

```json
{
    "faces": [
        {
            "age": 11,
            "gender": "Male",
            "faceRectangle": {
                "top": 62,
                "left": 22,
                "width": 45,
                "height": 45
            }
        },
        {
            "age": 11,
            "gender": "Female",
            "faceRectangle": {
                "top": 127,
                "left": 240,
                "width": 42,
                "height": 42
            }
        },
        {
            "age": 37,
            "gender": "Female",
            "faceRectangle": {
                "top": 55,
                "left": 200,
                "width": 41,
                "height": 41
            }
        },
        {
            "age": 41,
            "gender": "Male",
            "faceRectangle": {
                "top": 45,
                "left": 103,
                "width": 39,
                "height": 39
            }
        }
    ],
    "requestId": "3a383cbe-1a05-4104-9ce7-1b5cf352b239",
    "metadata": {
        "height": 230,
        "width": 300,
        "format": "Png"
    }
}
```

## <a name="next-steps"></a>后续步骤

有关如何使用人脸检测功能的详细信息，请参阅[分析图像](https://dev.cognitive.azure.cn/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fa)参考文档。

<!-- Update_Description: wording update -->