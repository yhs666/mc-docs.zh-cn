---
title: 对话听录 - 语音服务
titleSuffix: Azure Cognitive Services
description: 对话听录是语音服务的一项高级功能，它整合了实时语音识别、讲话人识别和分割聚类。 对话听录能够完美听录面对面会谈场景并可以区分讲话人，可让你知道谁在哪个时间讲了什么内容，使参与者能够专注于会议内容，快速跟进后续措施。 此功能还改进了辅助功能。 使用听录可以积极吸引有听力障碍的参与者。
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 07/05/2019
ms.date: 09/19/2019
ms.author: v-tawe
ms.openlocfilehash: 46507fb002e91c38609170f594539d502e0237b4
ms.sourcegitcommit: b328fdef5f35155562f10817af44f2a4e975c3aa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2019
ms.locfileid: "71267124"
---
# <a name="what-is-conversation-transcription"></a>什么是对话听录？

对话听录是语音服务的一项高级功能，它整合了实时语音识别、讲话人识别和分割聚类。 对话听录能够完美听录面对面会谈场景并可以区分讲话人，可让你知道谁在哪个时间讲了什么内容，使参与者能够专注于会议内容，快速跟进后续措施。 此功能还改进了辅助功能。 使用听录可以积极吸引有听力障碍的参与者。   

对话听录提供准确的识别结果和可定制的语音模型，可让你了解行业和公司特定的词汇。 此外，可将对话听录与语音设备 SDK 搭配使用，以优化多麦克风设备的体验。

>[!NOTE]
> 目前，建议将对话听录用于小型会议。 如果你想要将对话听录大规模扩展到大型会议，请联系我们。

下图演示了与对话听录结合使用的硬件、软件和服务。

![导入对话听录示意图](media/scenarios/conversation-transcription-service.png)

>[!IMPORTANT]
> 需要装配使用特定几何配置的 7 个麦克风的环形阵列。 有关规范和设计详细信息，请参阅 [Microsoft 语音设备 SDK 麦克风](speech-devices-sdk-microphone.md)。 若要详细了解或购买开发工具包，请参阅[获取 Microsoft 语音设备 SDK](get-speech-devices-sdk.md)。

## <a name="get-started-with-conversation-transcription"></a>开始使用对话听录

需要执行三个步骤才能开始使用对话听录。

1. 从用户收集语音样本。
2. 使用用户语音样本生成用户配置文件
3. 使用语音 SDK 确定用户（讲话人）并听录语音

## <a name="collect-user-voice-samples"></a>收集用户语音样本

第一步是从每个用户收集音频录制内容。 应在无背景噪声的安静环境中录制用户语音。 每个音频样本的建议长度为 30 秒到 2 分钟。 更长的音频样本可以在识别讲话人时提高准确度。 音频必须是 16 KHz 采样率的单声道音频。

除上述指导以外，有关如何录制和存储音频由你确定 -- 建议使用安全数据库。 下一部分将会介绍如何使用此音频来生成用户配置文件，供语音 SDK 用来识别讲话人。

## <a name="generate-user-profiles"></a>生成用户配置文件

接下来，需要将收集的音频录制内容发送到签名生成服务，以验证音频并生成用户配置文件。 [签名生成服务](https://aka.ms/cts/signaturegenservice)是用于生成和检索用户配置文件的一组 REST API。

若要创建用户配置文件，需要使用 `GenerateVoiceSignature` API。 有关规范详细信息和示例代码，请参阅：

<!--pending on service onboard-->

* [REST 规范](https://aka.ms/cts/signaturegenservice)
* [如何使用对话听录](how-to-use-conversation-transcription-service.md)

## <a name="transcribe-and-identify-speakers"></a>听录和识别讲话人

对话听录要求使用多声道音频流和用户配置文件作为输入来生成听录内容及识别讲话人。 音频和用户配置文件数据通过语音设备 SDK 发送到对话听录服务。 如前所述，若要使用对话听录，需要配备包含 7 个麦克风的环形阵列并安装语音设备 SDK。

>[!NOTE]
> 有关规范和设计详细信息，请参阅 [Microsoft 语音设备 SDK 麦克风](speech-devices-sdk-microphone.md)。 若要详细了解或购买开发工具包，请参阅[获取 Microsoft 语音设备 SDK](get-speech-devices-sdk.md)。

若要了解如何将对话听录与语音设备 SDK 配合使用，请参阅[如何使用对话听录](how-to-use-conversation-transcription-service.md)。


## <a name="quick-start-with-a-sample-app"></a>通过示例应用快速入门

Microsoft 语音设备 SDK 提供了一个快速入门示例应用用于演示所有设备相关的示例。 对话听录就是其中之一。 请参阅[语音设备 SDK Android 快速入门](speech-devices-sdk-android-quickstart.md)，其中提供了示例应用及其源代码供你参考。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [详细了解语音设备 SDK](speech-devices-sdk.md)
