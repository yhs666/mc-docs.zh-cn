---
title: 微调文本转语音输出 - 语音服务
titleSuffix: Azure Cognitive Services
description: 在语音 SDK 中启用日志记录。
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 07/05/2019
ms.date: 09/23/2019
ms.author: v-tawe
ms.openlocfilehash: 1c3a81bf97250e30cab36ab985b14be612852d70
ms.sourcegitcommit: b328fdef5f35155562f10817af44f2a4e975c3aa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2019
ms.locfileid: "71267126"
---
# <a name="fine-tune-text-to-speech-output"></a>微调文本转语音输出

Azure 语音服务允许使用[语音合成标记语言 (SSML)](speech-synthesis-markup.md) 调整文本转语音输出的语速、发音、音量、音节和调型。 SSML 是一种基于 XML 的标记语言，它使用标记来告知服务哪些特征需要优化。 然后，SSML 消息将在每个请求的正文中发送到文本转语音服务。 为了简化自定义过程，语音服务现在提供一个[语音优化](https://speech.microsoft.com/portal?projecttype=voicetuning)工具，让你实时直观检查和微调文本转语音输出。

语音优化工具支持 Microsoft 的[标准](language-support.md#standard-voices)、[神经](language-support.md#text-to-speech)和[自定义语音](how-to-custom-voice-create-voice.md)。

## <a name="get-started-with-the-voice-tuning-tool"></a>开始使用语音优化工具

在开始使用语音优化工具微调文本转语音输出之前，需要完成以下步骤：

1. 如果你没有 Azure 帐户，请创建一个[试用 Azure 帐户](https://www.azure.cn/pricing/1rmb-trial-full/?form-type=identityauth)。

2. 在 Azure 门户中创建语音服务订阅。 参阅有关[如何创建语音资源](get-started.md#create-a-speech-resource-in-azure)的分步说明。

    >[!NOTE]
    >在 Azure 门户中创建语音资源时，Azure 位置信息需要与 TTS 语音区域相匹配。 神经 TTS 语音支持一部分 Azure 位置。 有关完整的支持列表，请参阅[区域](regions.md#text-to-speech)。

    >[!NOTE]
    >在使用该服务之前，需要先在 Azure 门户中创建一个 F0 或 S0 密钥。

4. 登录到[语音优化](https://speech.microsoft.com/portal?projecttype=voicetuning)门户，并连接语音服务订阅。 选择单个语音服务订阅，然后创建一个项目。
5. 选择“新建优化”。  然后执行以下步骤：

   * 找到并选择“所有订阅”。   
   * 选择“连接现有订阅”  。  
     ![连接现有订阅](./media/custom-voice/custom-voice-connect-subscription.png)。
   * 输入 Azure 语音服务订阅密钥，然后选择“添加”。  语音自定义门户中的[“订阅”页](https://speech.microsoft.com/portal?noredirect=true)上提供了订阅密钥。 也可以从 [Azure 门户](https://portal.azure.cn/)中的“资源管理”窗格获取该密钥。
   * 如果你打算使用多个语音服务订阅，请对每个订阅重复上述步骤。

## <a name="customize-the-text-to-speech-output"></a>自定义文本转语音输出

创建帐户并链接订阅后，接下来可以开始优化文本转语音输出。

1. 选择语音。
2. 输入要编辑的文本。
3. 在开始编辑之前，请播放音频来大致了解输出结果。
4. 选择要优化的单词/句子，然后开始体验不同的基于 SSML 的功能。

>[!TIP]
> 有关调整 SSML 和优化语音输出的详细信息，请参阅[语音合成标记语言 (SSML)](speech-synthesis-markup.md)。

## <a name="limitations"></a>限制

神经语音的优化与标准和自定义语音的优化略有不同。

* 神经语音不支持语调。
* 音节和音量功能仅适用于完整的句子。 无法在单词级别使用这些功能。
* 在语速方面，可以根据单词优化某些神经语音，但还有一些神经语音需要选择整个句子。

> [!TIP]
> 语音优化工具提供有关特征和优化的上下文信息。

## <a name="next-steps"></a>后续步骤
* [在 Azure 中创建语音资源](get-started.md#create-a-speech-resource-in-azure)
* [开始优化语音](https://speech.microsoft.com/app.html#/VoiceTuning)
* [语音合成标记语言 (SSML)](speech-synthesis-markup.md)
