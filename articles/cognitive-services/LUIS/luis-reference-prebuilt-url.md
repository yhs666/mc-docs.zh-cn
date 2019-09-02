---
title: URL 预生成实体
titleSuffix: Azure
description: 本文包含语言理解 (LUIS) 中的 URL 预构建实体信息。
services: cognitive-services
author: lingliw
manager: digimobile
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 04/19/19
ms.author: v-lingwu
ms.openlocfilehash: f3682c80f7bb68b0f4badbfdee8f9015c41d0111
ms.sourcegitcommit: 68f7c41974143a8f7bd9b7a54acf41c09893e587
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/19/2019
ms.locfileid: "68332134"
---
# <a name="url-prebuilt-entity-for-a-luis-app"></a>LUIS 应用的 URL 预生成实体
URL 实体提取带域名或 IP 地址的 URL。 此实体已定型，因此不需要将包含 URL 的示例陈述添加到应用程序。 仅在 `en-us` 区域性中支持 URL 实体。 

## <a name="types-of-urls"></a>URL 类型
URL 托管在 [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-URL.yaml) GitHub 存储库

## <a name="resolution-for-prebuilt-url-entity"></a>预构建 URL 实体解析

### <a name="api-version-2x"></a>API 版本 2.x

以下示例演示了 builtin.url  实体解析。

```json
{
  "query": "https://luis.azure.cn is a great cognitive services example of artificial intelligence",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.781975448
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.781975448
    }
  ],
  "entities": [
    {
      "entity": "https://luis.azure.cn",
      "type": "builtin.url",
      "startIndex": 0,
      "endIndex": 17
    }
  ]
}
```

### <a name="preview-api-version-3x"></a>预览版 API 版本 3.x

以下 JSON 的 `verbose` 参数设置为 `false`：

```json
{
    "query": "https://luis.azure.cn is a great cognitive services example of artificial intelligence",
    "prediction": {
        "normalizedQuery": "https://luis.azure.cn is a great cognitive services example of artificial intelligence",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.421936184
            }
        },
        "entities": {
            "url": [
                "https://luis.azure.cn"
            ]
        }
    }
}
```

以下 JSON 的 `verbose` 参数设置为 `true`：

```json
{
    "query": "https://luis.azure.cn is a great cognitive services example of artificial intelligence",
    "prediction": {
        "normalizedQuery": "https://luis.azure.cn is a great cognitive services example of artificial intelligence",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.421936184
            }
        },
        "entities": {
            "url": [
                "https://luis.azure.cn"
            ],
            "$instance": {
                "url": [
                    {
                        "type": "builtin.url",
                        "text": "https://luis.azure.cn",
                        "startIndex": 0,
                        "length": 19,
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

了解[序号](luis-reference-prebuilt-ordinal.md)、[数字](luis-reference-prebuilt-number.md)和[温度](luis-reference-prebuilt-temperature.md)实体。




