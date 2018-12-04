---
title: 快速入门：识别图像中人脸的情感 - 情感 API、cURL
titlesuffix: Azure Cognitive Services
description: 获取信息和代码示例，以帮助你通过 cURL 快速开始使用情感 API。
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: quickstart
origin.date: 05/23/2017
ms.date: 10/24/2018
ms.author: v-junlch
ROBOTS: NOINDEX
ms.openlocfilehash: 0d57738f27a1d48ecd416657e5b12ae78aa3aaa0
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52666864"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>快速入门：构建应用以识别图像中人脸的情感。

> [!IMPORTANT]
> 情感 API 将于 2019 年 2 月 15 日弃用。 情感识别功能现在已作为[人脸 API](/cognitive-services/face/) 的一部分正式发布。

本文提供信息和代码示例，帮助读者快速开始使用[情感 API 识别方法](https://dev.cognitive.azure.cn/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)与 cURL 来识别图像中一个或多个人员表达的情感。

## <a name="prerequisite"></a>先决条件
- 从 [Azure 门户](https://portal.azure.cn)获取订阅密钥。

## <a name="recognize-emotions-curl-example-request"></a>识别情感 cURL 示例请求


```json
@ECHO OFF

curl -v -X POST "https://api.cognitive.azure.cn/emotion/v1.0/recognize"
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: {subscription key}"

--data-ascii "{body}"
```

## <a name="recognize-emotions-sample-response"></a>识别情感示例响应
成功调用返回人脸条目及其关联情感评分的数组，返回的内容已按人脸矩形大小的降序排序。 空响应表示未检测到任何人脸。 情感条目包含以下字段：
- faceRectangle - 人脸在图像中的矩形位置。
- scores - 图像中每张人脸的情感评分。

```json
application/json
[
  {
    "faceRectangle": {
      "left": 68,
      "top": 97,
      "width": 64,
      "height": 97
    },
    "scores": {
      "anger": 0.00300731952,
      "contempt": 5.14648448E-08,
      "disgust": 9.180124E-06,
      "fear": 0.0001912825,
      "happiness": 0.9875571,
      "neutral": 0.0009861537,
      "sadness": 1.889955E-05,
      "surprise": 0.008229999
    }
  }
]

