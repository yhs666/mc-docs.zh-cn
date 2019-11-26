---
title: 数据转换 - LUIS
titleSuffix: Azure Cognitive Services
description: 了解如何在语言理解 (LUIS) 得出预测之前更改话语
services: cognitive-services
author: lingliw
manager: digimobile
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
origin.date: 07/29/2019
ms.date: 09/23/2019
ms.author: v-lingwu
ms.openlocfilehash: 4a27f4b7b2789fbfc9398db9cae8c98e09f70399
ms.sourcegitcommit: 9e92bcf6aa02fc9e7b3a29abadf6b6d1a8ece8c4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74389434"
---
# <a name="convert-data-format-of-utterances"></a>转换话语的数据格式
LUIS 在预测之前提供用户话语的以下转换

* 语音转文本（使用[认知服务语音](../Speech-Service/overview.md)服务）。 

<!-- ## Speech to text -->

<!-- Speech to text is provided as an integration with LUIS.  -->

<!-- ### Intent conversion concepts -->
<!-- Conversion of speech to text in LUIS allows you to send spoken utterances to an endpoint and receive a LUIS prediction response. The process is an integration of the [Speech](https://docs.microsoft.com/azure/cognitive-services/Speech) service with LUIS. Learn more about Speech to Intent with a [tutorial](../speech-service/how-to-recognize-intents-from-speech-csharp.md). -->

### <a name="key-requirements"></a>关键要求
无需为此集成创建必应语音 API 密钥  。 可在此集成中使用 Azure 门户中创建的语言理解密钥  。 请勿使用 LUIS 初学者密钥。

### <a name="pricing-tier"></a>定价层
此集成使用与通常的语言理解定价层不同的[定价](luis-boundaries.md#key-limits)模型。 

### <a name="quota-usage"></a>配额使用情况
相关信息请参阅[密钥限制](luis-boundaries.md#key-limits)。 

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [提取数据](luis-concept-data-extraction.md)





