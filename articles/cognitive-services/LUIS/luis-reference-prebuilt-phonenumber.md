---
title: 电话号码预构建实体
titleSuffix: Azure
description: 本文包含语言理解 (LUIS) 中的电话号码预构建实体信息。
services: cognitive-services
author: lingliw
manager: digimobile
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 04/19/19
ms.author: v-lingwu
ms.openlocfilehash: c4b796f67f183e8aeef2e3ed54f816f98b16af9d
ms.sourcegitcommit: 71ec68c5d696abd9704363e26d09a80afed2c7a6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/17/2019
ms.locfileid: "65828511"
---
# <a name="phonenumber-prebuilt-entity-for-a-luis-app"></a>LUIS 应用的 Phonenumber 预构建实体
`phonenumber` 实体提取各种包括国家/地区代码在内的电话号码。 此实体已定型，因此不需要将陈述示例添加到应用程序。 仅在 `en-us` 区域性中支持 `phonenumber` 实体。 

## <a name="types-of-a-phone-number"></a>电话号码的类型
`Phonenumber` 托管在 [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-PhoneNumbers.yaml) GitHub 存储库中

## <a name="resolution-for-this-prebuilt-entity"></a>针对此预生成实体的解决方法

### <a name="api-version-2x"></a>API 版本 2.x

以下示例演示了 builtin.phonenumber 实体解析。

```json
{
  "query": "my mobile is 1 (800) 642-7676",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.8448457
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.8448457
    }
  ],
  "entities": [
    {
        "entity": "1 (800) 642-7676",
        "type": "builtin.phonenumber",
        "startIndex": 13,
        "endIndex": 28,
        "resolution": {
            "score": "1",
            "value": "1 (800) 642-7676"
        }
    }
  ]
}
```

### <a name="preview-api-version-3x"></a>预览版 API 版本 3.x

以下 JSON 的 `verbose` 参数设置为 `false`：

```json
{
    "query": "my mobile is 1 (800) 642-7676",
    "prediction": {
        "normalizedQuery": "my mobile is 1 (800) 642-7676",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.592748761
            }
        },
        "entities": {
            "phonenumber": [
                "1 (800) 642-7676"
            ]
        }
    }
}
```

以下 JSON 的 `verbose` 参数设置为 `true`：

```json
{
    "query": "my mobile is 1 (800) 642-7676",
    "prediction": {
        "normalizedQuery": "my mobile is 1 (800) 642-7676",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.592748761
            }
        },
        "entities": {
            "phonenumber": [
                "1 (800) 642-7676"
            ],
            "$instance": {
                "phonenumber": [
                    {
                        "type": "builtin.phonenumber",
                        "text": "1 (800) 642-7676",
                        "startIndex": 13,
                        "length": 16,
                        "score": 1,
                        "modelTypeId": 2,
                        "modelType": "Prebuilt Entity Extractor"
                    }
                ]
            }
        }
    }
}
```

## <a name="next-steps"></a>后续步骤

了解有关[百分比](luis-reference-prebuilt-percentage.md)、[数字](luis-reference-prebuilt-number.md)和[温度](luis-reference-prebuilt-temperature.md)实体。 




