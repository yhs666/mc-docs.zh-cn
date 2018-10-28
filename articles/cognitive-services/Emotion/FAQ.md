---
title: 常见问题解答 - 情感 API
titlesuffix: Azure Cognitive Services
description: 获取有关情感 API 的常见问题的解答。
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: conceptual
origin.date: 01/26/2017
ms.date: 10/25/2018
ms.author: v-junlch
ROBOTS: NOINDEX
ms.openlocfilehash: 188066cb9f7a01ff5244f5be6fb5cc420aed22f9
ms.sourcegitcommit: 44ce337717bb948f5ac08217a156935f663c0f46
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2018
ms.locfileid: "50034673"
---
# <a name="emotion-api-frequently-asked-questions"></a>情感 API 常见问题解答

> [!IMPORTANT]
> 情感 API 将于 2019 年 2 月 15 日弃用。 情感识别功能现在已作为[人脸 API](/cognitive-services/face/) 的一部分正式发布。

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-emotion-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>如果在本“常见问题解答”中找不到问题的答案，请尝试在 [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) 中向情感 API 社区提问，或联系 [UserVoice 上的帮助和支持人员](https://cognitive.uservoice.com/)。  

-----

**问**：*情感 API 能够从哪些类型的图像中返回最佳结果？*

**答**：使用无遮挡、完整的正面人脸图像可以获取最佳结果。 如果是部分正面人脸，可靠性会下降。如果图像中的人脸旋转角度大于 45 度，情感 API 可能无法识别情感。

-----

**问**：*情感 API 可以识别多少种情感？*

**答**：情感 API 可以识别 8 种公认的不同情感：
- 快乐
- 悲伤
- 惊讶
- 愤怒
- 恐惧
- 蔑视
- 厌恶
- 中性

-----

**问**：*是否有任何方法可将实时视频流传递到该 API 并同时获取结果？*

**答**：使用基于图像的情感 API，并针对每个帧调用该 API，或跳帧以提高性能。  我们提供了视频逐帧分析示例。

-----

**问**：*我正在传入二进制图像数据，但却看到“人脸图像无效”错误消息。**

**答**：此消息表明，算法无法分析图像。  
- 支持的输入图像格式包括 JPEG、PNG、GIF（第一帧）和 BMP
- 图像文件不得大于 4 MB
- 可检测的人脸大小范围为 36 x 36 到 4096 x 4096 像素。 无法检测超出此范围的人脸
- 某些面部可能因技术难题而无法检测，例如非常大的面部角度（头部姿势），以及较大的阻挡物。 正面和接近正面的人脸可提供最佳效果

-----

