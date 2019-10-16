---
title: 有关语音设备 SDK - 语音服务
titleSuffix: Azure Cognitive Services
description: 语音设备 SDK 入门。 “语音服务”适用于多种设备和音频源。 现在，可以通过匹配的硬件和软件进一步利用语音应用程序。 语音设备 SDK 是与特制的麦克风阵列开发工具包配对的一个预优化库。
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 07/05/2019
ms.date: 09/23/2019
ms.author: v-tawe
ms.openlocfilehash: 9445631d2a415982ec961083d75b1c3b5a64ef91
ms.sourcegitcommit: b328fdef5f35155562f10817af44f2a4e975c3aa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2019
ms.locfileid: "71267039"
---
# <a name="about-the-speech-devices-sdk"></a>关于语音设备 SDK

[语音服务](overview.md)适用于多种设备和音频源。 现在，可以通过匹配的硬件和软件进一步利用语音应用程序。 语音设备 SDK 是与特制的麦克风阵列开发工具包配对的一个预优化库。

语音设备 SDK 有助于：

* 快速测试新的语音方案。
* 更轻松地将基于云的语音服务集成到设备中。
* 为客户创建出色的用户体验。

语音设备 SDK 使用[语音 SDK](speech-sdk.md)。 它使用语音 SDK 将我们的高级音频处理算法处理的音频从设备的麦克风阵列发送到[语音服务](overview.md)。

你还可以使用语音设备 SDK 来构建具有你自己的[自定义唤醒字](speech-devices-sdk-create-kws.md)的环境设备，因此启动用户交互的提示词将为品牌特有。

该语音设备 SDK 支持多种启用语音的方案，例如免下车点单系统、[对话听录](conversation-transcription-service.md)和智能扬声器。 可以使用文本回复用户、用默认或[自定义语音](how-to-custom-voice-create-voice.md)回答用户、提供搜索结果等。 我们期待看到你的成果！

## <a name="get-the-speech-devices-sdk"></a>获取语音设备 SDK

### <a name="android"></a>Android

对于 Android 设备，请下载最新版本的 [Android 语音设备 SDK](https://aka.ms/sdsdk-download-android)。

### <a name="windows"></a>Windows

对于 Windows，示例应用程序以跨平台 Java 应用程序的形式提供。 下载最新版本的 [JRE 语音设备 SDK](https://aka.ms/sdsdk-download-JRE)。
该应用程序是使用语音 SDK 程序包和 Eclipse Java IDE (v4) 在 64 位 Windows 上构建的。 它在 64 位 Java 8 运行时环境 (JRE) 中运行。

### <a name="linux"></a>Linux

对于 Linux，示例应用程序以跨平台 Java 应用程序的形式提供。 下载最新版本的 [JRE 语音设备 SDK](https://aka.ms/sdsdk-download-JRE)。
该应用程序是使用语音 SDK 程序包和 Eclipse Java IDE (v4) 在 64 位 Linux（Ubuntu 16.04、Ubuntu 18.04、Debian 9）上构建的。 它在 64 位 Java 8 运行时环境 (JRE) 中运行。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [选择语音设备](get-speech-devices-sdk.md)
>
> [!div class="nextstepaction"]
> [免费获取语音服务订阅密钥](get-started.md)
