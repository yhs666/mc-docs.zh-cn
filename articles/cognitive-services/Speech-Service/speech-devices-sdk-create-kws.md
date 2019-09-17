---
title: 创建自定义唤醒词 - 语音服务
titleSuffix: Azure Cognitive Services
description: 设备始终在侦听唤醒词（或短语）。 当用户说唤醒词时，设备会将所有后续音频发送到云，直到用户停止说话为止。 自定义唤醒词是区分设备和加强品牌效应的有效方式。
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 12/06/2018
ms.date: 07/05/2019
ms.author: v-biyu
ms.custom: seodec18
ms.openlocfilehash: 2d97007bfe011130a8d841351375a08b6308ca7f
ms.sourcegitcommit: 13642a99cc524a416b40635f48676bbf5cdcdf3d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2019
ms.locfileid: "70104128"
---
# <a name="create-a-custom-wake-word-by-using-the-speech-service"></a>使用语音服务创建自定义唤醒词

设备始终在侦听唤醒词（或短语）。 例如，“Hey Cortana”是 Cortana 助手的唤醒词。 当用户说唤醒词时，设备会将所有后续音频发送到云，直到用户停止说话为止。 自定义唤醒词是区分设备和加强品牌效应的有效方式。

本文介绍如何为设备创建自定义唤醒词。

## <a name="choose-an-effective-wake-word"></a>选择有效的唤醒词

选择唤醒词时请考虑以下准则：

* 唤醒词应该是英文单词或短语。 说该词应该不超过两秒钟。

* 4 到 7 个音节的单词效果最好。 例如，“Hey, Computer”是一个很好的唤醒词， 而只是“Hey”则是一个糟糕的唤醒词。

* 唤醒词应遵循常见的英语发音规则。

* 遵循常见英语发音规则的独特或甚至虚构的单词可以减少误报。 例如，“computerama”可能是一个很好的唤醒词。

* 不要选择常用词。 例如，“eat”和“go”是人们在日常对话中经常说的词。 它们可能是设备的错误触发器。

* 避免使用可能有其他发音的唤醒词。 用户必须知道“正确”的发音才能使他们的设备做出响应。 例如，“509”的发音可以是“five zero nine”、“five oh nine”或“five hundred and nine”。 “R.E.I.” 发音可以是“r-e-i”或“ray”。 “Live”的发音可以是“/līv/”或“/liv/”。

* 不要使用特殊字符、符号或数字。 例如，“Go#”和“20 + cats”不会是好的唤醒词。 但是，“go sharp”或“twenty plus cats”可行。 你仍然可以在品牌中使用这些符号，并通过营销和文档来强化正确的发音。

> [!NOTE]
> 如果选择商标词作为唤醒词，请确保你拥有该商标，或者获得商标所有者的许可才能使用该词。 Microsoft 对你选择的唤醒词可能引起的任何法律问题不承担任何责任。

## <a name="create-your-wake-word"></a>创建唤醒词

在设备上使用自定义唤醒词之前，需先使用 Microsoft 自定义唤醒词生成服务来创建唤醒词。 你提供唤醒词后，该服务会生成一个文件，由你将该文件部署到开发工具包中，以便在设备上启用唤醒词。

1. 转到[自定义语音服务门户](https://aka.ms/sdsdk-speechportal)并**登录**，或者，如果没有语音订阅，请选择[**创建订阅**](https://go.microsoft.com/fwlink/?linkid=2086754)

    ![自定义语音服务门户](media/speech-devices-sdk/wake-word-4.png)

1. 在[自定义唤醒词](https://aka.ms/sdsdk-wakewordportal)页上，键入所选的唤醒词，然后单击“添加唤醒词”。  我们有一些[准则](#choose-an-effective-wake-word)，可以帮助你选择有效的关键字。 目前，我们仅支持 en-US 语言。

    ![输入唤醒词](media/speech-devices-sdk/wake-word-5.png)

1. 将会创建三种其他发音的唤醒词。 可以选择自己喜欢的所有发音。 然后选择“提交”，生成唤醒词。  若要更改唤醒词，请先删除现有的唤醒词。将鼠标悬停在发音行上时，会显示删除图标。

    ![查看唤醒词](media/speech-devices-sdk/wake-word-6.png)

1. 生成模型最长可能需要一分钟时间。 系统会提示你下载该文件。

    ![下载唤醒词](media/speech-devices-sdk/wake-word-7.png)

1. 将 .zip 文件保存到计算机。 你需要此文件才能将自定义唤醒词部署到开发工具包。


