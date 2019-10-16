---
title: 语音设备 SDK 文档
titleSuffix: Azure Cognitive Services
description: 发行说明 - 最新版本中的内容更改
services: cognitive-services
author: wsturman
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 07/10/2019
ms.date: 09/19/2019
ms.author: v-tawe
ms.openlocfilehash: a020fd92a0fccde14e10866de12d6bf6dd222fbc
ms.sourcegitcommit: b328fdef5f35155562f10817af44f2a4e975c3aa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2019
ms.locfileid: "71267061"
---
# <a name="release-notes-of-cognitive-services-speech-devices-sdk"></a>认知服务语音设备 SDK 发行说明
以下部分列出了最新版本中的更改。

## <a name="speech-devices-sdk-160"></a>语音设备 SDK 1.6.0：
*   已将[语音 SDK](speech-sdk.md) 组件更新到 1.6.0 版。 有关详细信息，请参阅其[发行说明](releasenotes.md)。

## <a name="speech-devices-sdk-151"></a>语音设备 SDK 1.5.1：

*   在示例应用中包括[对话听录](conversation-transcription-service.md)。
*   已将[语音 SDK](speech-sdk.md) 组件更新到 1.5.1 版。 有关详细信息，请参阅其[发行说明](releasenotes.md)。

## <a name="cognitive-services-speech-devices-sdk-150-2019-may-release"></a>认知服务语音设备 SDK 1.5.0：2019 年 5 月发布

*   语音设备 SDK 现为 GA，不再是受限的预览版。
*   已将[语音 SDK](speech-sdk.md) 组件更新到 1.5.0 版。 有关详细信息，请参阅其[发行说明](releasenotes.md)。
*   新的唤醒词技术带来了显著的质量改进，请参阅“中断性变更”。
*   新的音频处理管道改进了远端域识别。

**重大更改**

*   由于新出现的唤醒词技术，所有唤醒词必须在改进的唤醒词门户中重新创建。 若要从设备中完全删除旧的关键字，请卸载旧应用。
    - adb uninstall com.microsoft.coginitiveservices.speech.samples.sdsdkstarterapp

## <a name="cognitive-services-speech-devices-sdk-140-2019-apr-release"></a>认知服务语音设备 SDK 1.4.0：2019 年 4 月发布

* 已将[语音 SDK](speech-sdk.md) 组件更新到 1.4.0 版。 有关详细信息，请参阅其[发行说明](releasenotes.md)。

## <a name="cognitive-services-speech-devices-sdk-131-2019-mar-release"></a>认知服务语音设备 SDK 1.3.1：2019 年 3 月发布

* 已将[语音 SDK](speech-sdk.md) 组件更新到 1.3.1 版。 有关详细信息，请参阅其[发行说明](releasenotes.md)。
*   更新了唤醒词处理，请参阅“中断性变更”。

**重大更改**

*   [安装唤醒词](speech-devices-sdk-create-kws.md)的过程已简化，它现在已是应用的一部分，不需在设备上单独安装。
*   唤醒词识别已更改，支持两个事件。
    - RecognizingKeyword，指示语音结果包含（未验证的）关键字文本。
    - RecognizedKeyword，指示关键字识别已完成识别给定关键字的过程。


## <a name="cognitive-services-speech-devices-sdk-110-2018-nov-release"></a>认知服务语音设备 SDK 1.1.0：2018 年 11 月版本

* 已将[语音 SDK](releasenotes.md) 组件更新到 1.1.0 版。 有关详细信息，请参阅其[发行说明](releasenotes.md)。 
* 远场语音识别准确性已通过我们增强的音频处理算法得到提高。
* 示例应用程序添加了中文语音识别支持。

## <a name="cognitive-services-speech-devices-sdk-101-2018-oct-release"></a>认知服务语音设备 SDK 1.0.1：2018 年 10 月版本

* 将[语音 SDK](releasenotes.md) 组件更新到了 1.0.1 版。 有关详细信息，请参阅其[发行说明](releasenotes.md)。 
* 我们改进的音频处理算法将提高语音识别的准确性  
* 修复了一个连续识别音频会话 bug。

**重大更改**

* 该版本中推出了大量重大更改。 有关 API 的详细信息，请查看[此页](https://aka.ms/csspeech/breakingchanges_1_0_0)。
* KWS 模型文件与语音设备 SDK 1.0.1 不兼容。 将新的唤醒词文件写入设备后，将删除现有的唤醒词文件。

## <a name="cognitive-services-speech-devices-sdk-050-2018-aug-release"></a>认知服务语音设备 SDK 0.5.0：2018 年 8 月版本

* 通过修复音频处理代码中的 Bug，改进了语音识别的准确性。
* 将[语音 SDK](speech-sdk.md) 组件更新到了 0.5.0 版。 有关详细信息，请参阅其[发行说明](releasenotes.md#cognitive-services-speech-sdk-050-2018-july-release)。

## <a name="cognitive-services-speech-devices-sdk-0212733-2018-may-release"></a>认知服务语音设备 SDK 0.2.12733：2018 年 5 月版本

认知服务语音设备 SDK 的第一个公共预览版本。
