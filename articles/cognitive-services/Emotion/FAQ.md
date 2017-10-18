---
title: "情感 API 常见问题解答 | Microsoft Docs"
description: "获取有关认知服务中情感 API 的常见问题的解答。"
services: cognitive-services
author: alexchen2016
manager: digimobile
ms.service: cognitive-services
ms.technology: emotion
ms.topic: article
origin.date: 01/26/2017
ms.date: 10/13/2017
ms.author: v-junlch
ms.openlocfilehash: 793517343ecdc402109dca0489806db983280121
ms.sourcegitcommit: 9b2b3a5aede3a66aaa5453e027f1e7a56a022d49
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2017
---
# <a name="emotion-api-frequently-asked-questions"></a>情感 API 常见问题解答
### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-emotion-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>如果在本“常见问题解答”中找不到问题的答案，请尝试在 [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) 中向情感 API 社区提问，或联系 [UserVoice 上的帮助和支持人员](https://cognitive.uservoice.com/)。  

-----

**问**：*情感 API 能够从哪些类型的图像中返回最佳结果？*

**答**：使用无遮挡、完整的正面人脸图像可以获取最佳结果。 使用不完整的正面人脸图像会降低可靠性，在人脸被旋转 45 度或以上的图像中，情感 API 可能识别不到情感。

-----

**问**：*情感 API 可以识别多少种情感？*

**答**：情感 API 识别八个不同的、全球认可的情感： 
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

**问**：*我正在传入二进制图像数据，但出现消息：“人脸图像无效”。**

**答**：这意味着算法在分析图像时出现问题。  
- 支持的输入图像格式包括 JPEG、PNG、GIF（第一帧）和 BMP。 
- 图像文件的大小不应超过 4MB
- 可检测的面部大小范围为 36x36 到 4096x4096 像素。 不会检测超过此范围的人脸
- 某些人脸可能因技术难题而无法检测，例如非常大的人脸角度（头部姿势），以及较大的阻挡物。 正面和接近正面的人脸可提供最佳效果

-----

