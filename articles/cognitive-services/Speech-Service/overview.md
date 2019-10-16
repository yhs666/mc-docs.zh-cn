---
title: 什么是语音服务？
titleSuffix: Azure Cognitive Services
description: 语音服务将文本转语音统合到单个 Azure 订阅。 使用语音 SDK、语音设备 SDK 或 REST API 可以轻松在应用程序、工具和设备中添加语音。 可将语音功能添加到现有的聊天机器人，在翻译应用程序中将文本转换为语音，或者听录大量的呼叫中心数据。
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: overview
origin.date: 07/05/2019
ms.date: 09/23/2019
ms.author: v-tawe
ms.openlocfilehash: 447627bacb062db716f40b1553c675c39aff7c1a
ms.sourcegitcommit: b328fdef5f35155562f10817af44f2a4e975c3aa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2019
ms.locfileid: "71267051"
---
# <a name="what-are-the-speech-services"></a>什么是语音服务？

语音服务将“文本转语音”统合到单个 Azure 订阅。 使用[语音 SDK](speech-sdk.md)、[语音设备 SDK](speech-devices-sdk-android-quickstart.md) 或 [REST API](#reference-docs) 可以轻松在应用程序、工具和设备中启用语音。

> [!IMPORTANT]
> 语音服务已替代必应语音 API、语音翻译和自定义语音。 有关迁移说明，请参阅*操作指南 > 迁移*。

这些功能构成了 Azure 语音服务。 请使用下表中的链接详细了解每项功能的常见用例或浏览 API 参考信息。

| 服务 | 功能 | 说明 | SDK | REST |
|---------|---------|-------------|-----|------|
| | [对话听录](conversation-transcription-service.md) | 启用实时语音识别、说话人识别和分割聚类。 它非常适合用于听录能够区分说话人的面对面会谈场景。 | 是 | 否 |
| [文本转语音](text-to-speech.md) | 文本转语音 | 文本转语音可使用[语音合成标记语言 (SSML)](text-to-speech.md#speech-synthesis-markup-language-ssml) 将输入文本转换为类似人类的合成语音。 可以选择标准语音或神经语音（请参阅[语言支持](language-support.md)）。 | [是](speech-sdk.md) | [是](overview.md) |
| | [创建自定义语音](#customize-your-speech-experience) | 创建专属于品牌或产品的自定义语音字体。 | 否 | [是](overview.md) |

<!-- voice first/bot service not available -->
<!-- | [Voice-first Virtual Assistants](voice-first-virtual-assistants.md) | Voice-first virtual assistants | Custom virtual assistants using Azure Speech Services empower developers to create natural, human-like conversational interfaces for their applications and experiences. The Bot Framework's Direct Line Speech channel enhances these capabilities by providing a coordinated, orchestrated entry point to a compatible bot that enables voice in, voice out interaction with low latency and high reliability. | [Yes](voice-first-virtual-assistants.md) | No | -->

## <a name="news-and-updates"></a>新增功能和更新

了解 Azure 语音服务的新增功能。

<!-- voice first/bot service not available -->

* 2019 年 8 月
  * 添加了一种新的说话风格 [`chat`](speech-synthesis-markup.md#adjust-speaking-styles)，用于 `en-US-JessaNeural` 语音。 
* 2019 年 7 月
  * 发布了语音 SDK 1.6.0。 有关更新、增强功能和已知问题的完整列表，请参阅[发行说明](releasenotes.md)。
* 2019 年 5 月
  * 发布了语音 SDK 1.5.1。 有关更新、增强功能和已知问题的完整列表，请参阅[发行说明](releasenotes.md)。
  * 发布了语音 SDK 1.5.0。 有关更新、增强功能和已知问题的完整列表，请参阅[发行说明](releasenotes.md)。

## <a name="try-speech-services"></a>试用语音服务

我们提供了适用于大多数流行编程语言的快速入门，旨在帮助你在 10 分钟以内运行代码。 下表包含有关每项功能在最流行编程语言中的用法的快速入门。 使用左侧的导航栏可以浏览其他语言和平台。

| 文本转语音 (SDK) |
|----------------------|
| [C#、.NET Framework (Windows)](quickstart-text-to-speech-dotnet-windows.md) |
| [C++ (Windows)](quickstart-text-to-speech-cpp-windows.md) |
| [C++ (Linux)](quickstart-text-to-speech-cpp-linux.md) |

> [!NOTE]
> “文本转语音”也有 REST 终结点和关联的快速入门。

有机会使用语音服务后，请尝试学习有关如何使用语音 SDK 和 LUIS 从语音中识别意向的教程。

* [教程：生成 Flask 应用以翻译文本、分析情绪以及将翻译后的文本合成为语音 - REST](https://docs.azure.cn/cognitive-services/translator/tutorial-build-flask-app-translation-synthesis?toc=%2fazure%2fcognitive-services%2fspeech-service%2ftoc.json&bc=%2fazure%2fcognitive-services%2fspeech-service%2fbreadcrumb%2ftoc.json&toc=%2Fen-us%2Fazure%2Fcognitive-services%2Fspeech-service%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)

## <a name="get-sample-code"></a>获取示例代码

GitHub 中提供了每个 Azure 语音服务的示例代码。 这些示例涵盖了常见方案，例如，从文件或流中读取音频、连续和单次识别，以及使用自定义模型。 使用以下链接查看 SDK 和 REST 示例：

* [批量听录示例 (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)
* [文本转语音示例 (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)

## <a name="customize-your-speech-experience"></a>自定义语音体验

Azure 语音服务能够很好地与内置模型配合工作，但是，你可能想要根据自己的产品或环境，进一步自定义和优化体验。 自定义选项的范围从声学模型优化，到专属于自有品牌的语音字体。 生成自定义模型后，可将其与任何 Azure 语音服务配合使用。

| 语音服务 | 平台 | 说明 |
|----------------|-------------|-------------|
| 文本转语音 | [自定义语音](https://speech.microsoft.com/customvoice) | 使用可用语音数据为文本转语音应用生成可识别的独一无二的语音。 可以通过调整一组语音参数来进一步微调语音输出。 |

## <a name="reference-docs"></a>参考文档

* [语音 SDK](speech-sdk.md)
* [语音设备 SDK](speech-devices-sdk.md)
* [REST API：文本转语音](rest-text-to-speech.md)
* [REST API：批量听录和自定义](https://westus.cris.ai/swagger/ui/index)

<!-- this westus apps is a sample code websites -->

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [免费获取语音服务订阅密钥](get-started.md)
