---
title: 语音转文本 - 语音服务
titleSuffix: Azure Cognitive Services
description: 使用语音服务中的语音转文本功能，可将音频流实时听录成可由应用程序、工具或设备根据命令输入使用、显示和处理的文本。 此服务以 Microsoft 对 Cortana 和 Office 产品使用的相同识别技术为后盾，可与翻译和文本转语音功能无缝配合。
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 08/08/2019
ms.date: 07/05/2019
ms.author: v-lingwu
ms.openlocfilehash: 225e2990f357711d7949adc4aa9770fba0fd7290
ms.sourcegitcommit: 13642a99cc524a416b40635f48676bbf5cdcdf3d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2019
ms.locfileid: "70104119"
---
# <a name="what-is-speech-to-text"></a>什么是语音转文本？

使用 Azure 语音服务中的语音转文本功能，可将音频流实时听录成可由应用程序、工具或设备根据命令输入使用、显示和处理的文本。 此服务以 Microsoft 对 Cortana 和 Office 产品使用的相同识别技术为后盾，可与翻译和文本转语音功能无缝配合。  有关可用语音转文本语言的完整列表，请参阅[支持的语言](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#speech-to-text)。

默认情况下，语音转文本服务使用通用语言模型。 此模型已使用 Microsoft 自有的数据训练，部署在云中。 它非常适合用于对话和听写方案。 如果使用语音转文本在独特的环境中进行识别和听录，则可以创建并训练自定义的声学、语言和发音模型，以解决环境干扰或行业特定的词汇。

可以轻松通过麦克风捕获音频，读取流中的数据，或使用语音 SDK 和 REST API 访问存储中的音频文件。 语音 SDK 支持在 WAV/PCM 16 位 16 kHz/8 kHz 单声道音频中进行语音识别。 可以使用[语音转文本 REST 终结点](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis)或[批量听录服务](https://docs.microsoft.com/azure/cognitive-services/speech-service/batch-transcription#supported-formats)支持其他音频格式。

## <a name="core-features"></a>核心功能

下面是可以通过语音 SDK 和 REST API 获得的功能：

| 使用案例 | SDK | REST |
|----------|-----|------|
| 听录简短言语（15 秒以下）。 仅支持最终听录结果。 | 是 | 是 |
| 持续听录较长言语和流音频（15 秒以上）。 支持临时和最终听录结果。 | 是 | 否 |
| 使用 [LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/what-is-luis) 从识别结果中派生意向。 | 是 | 否\* |
| 以异步方式批量听录音频文件。 | 否 | 是\** |
| 创建和管理语音模型。 | 否 | 是\** |
| 创建和管理自定义模型部署。 | 否 | 是\** |
| 创建准确度测试，以测量基线模型与自定义模型的准确度。 | 否 | 是\** |
| 管理订阅。 | 否 | 是\** |

\* *可以使用单独的 LUIS 订阅获取 LUIS 意向和实体。通过此订阅，SDK 可为你调用 LUIS 并提供实体和意向结果。使用 REST API，可以自己调用 LUIS 以使用 LUIS 订阅获取意向和实体。*

\** *可以通过 cris.ai 终结点使用这些服务。请参阅 [Swagger 参考](https://westus.cris.ai/swagger/ui/index)。*

## <a name="get-started-with-speech-to-text"></a>语音转文本入门

我们提供了适用于大多数流行编程语言的快速入门，旨在帮助你在 10 分钟以内运行代码。 [此表](https://aka.ms/csspeech#5-minute-quickstarts)包含根据平台和语言组织的语音 SDK 快速入门完整列表。  还可以在[此处](https://aka.ms/csspeech#reference)找到 API 参考。

如果你偏向于使用语音转文本 REST 服务，请参阅 [REST API](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis)。

## <a name="tutorials-and-sample-code"></a>教程和示例代码

有机会使用语音服务后，请尝试学习有关如何使用语音 SDK 和 LUIS 从语音中识别意向的教程。

* [教程：使用适用于 C# 的语音 SDK 和 LUIS 从语音中识别意向](how-to-recognize-intents-from-speech-csharp.md)

GitHub 上提供了语音 SDK 的示例代码。 这些示例涵盖了常见方案，例如，从文件或流中读取音频、连续和单次识别，以及使用自定义模型。

* [语音转文本示例 (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
* [批量听录示例 (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)

## <a name="customization"></a>自定义

除了语音服务使用的通用模型以外，你还可以创建特定于体验的自定义声学模型、语言模型和发音模型。 下面是自定义选项的列表：

| 型号 | 说明 |
|-------|-------------|
| 声学模型 | 如果在特定的环境（例如汽车，或者具备特定录制条件的工厂）中使用应用程序、工具或设备，则创建自定义声学模型会很有帮助。 示例包括带有口音的讲话、特定的背景噪音，或使用特定的麦克风录制音频。 |
| 语言模型 | 创建自定义语言模型来改善行业特定的词汇和语法的听录，例如医疗术语中或 IT 行话。 |
| 发音模型 | 借助自定义发音模型，可以定义语音形式以及字词或术语的显示。 它适用于处理自定义术语，如产品名称或首字母缩略词。 只需使用发音文件（简单的 .txt 文件）即可。 |

> [!NOTE]
> 自定义选项因语言/区域设置而异（请参阅[支持的语言](supported-languages.md)）。

## <a name="migration-guides"></a>迁移指南

> [!WARNING]
> 必应语音将在 2019 年 10 月 15 日退役。

如果你的应用程序、工具或产品正在使用必应语音 API 或自定义语音，请参阅我们制作的指南迁移到语音服务。


## <a name="next-steps"></a>后续步骤

* [获取语音试用订阅](https://www.azure.cn/zh-cn/home/features/cognitive-services/)
* [快速入门：在 C# 中识别语音](quickstart-csharp-dotnet-windows.md)

