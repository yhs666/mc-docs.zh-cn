---
title: 常见问题解答 - 人脸 API
titlesuffix: Azure Cognitive Services
description: 下面是有关人脸 API 服务的最常见问题的解答。
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: conceptual
origin.date: 01/26/2017
ms.date: 10/24/2018
ms.author: v-junlch
ms.openlocfilehash: 709cafee69bfb9e6501f23b7b148e46976f033b9
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52644349"
---
# <a name="face-api-frequently-asked-questions"></a>人脸 API 常见问题解答

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-face-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>如果在本“常见问题解答”中找不到问题的答案，请尝试在 [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) 中向人脸 API 社区提问，或联系 [UserVoice 上的帮助和支持人员](https://cognitive.uservoice.com/)。

-----
**问**：哪些因素可能会降低人脸 API 的识别、验证或查找相似人脸功能的准确性？

**答**：通常，这些因素与人们难以识别某人的原因相同，包括：
- 有障碍物挡住了一只或两只眼睛
- 照明极差，例如有严重的背光
- 发型或面部毛发发生变化
- 由于年龄不同而发生变化
- 极端的面部表情（例如尖叫）

人脸 API 通常能够成功应对此类难题，但准确性可能降低。 若要提高识别的可靠性并解决这些难题，请使用包含各种角度和照明的照片训练“人员”模型。

-----
**问**：我正在传入二进制图像数据，但出现了“人脸图像无效”错误。

**答**：这意味着算法在分析图像时出现问题。 原因包括：
- 支持的输入图像格式包括 JPEG、PNG、GIF（第一帧）和 BMP。
- 图像文件的大小不应超过 4MB
- 可检测的面部大小范围为 36x36 到 4096x4096 像素。 不会检测超过此范围的人脸
- 某些人脸可能因技术难题而无法检测，例如非常大的人脸角度（头部姿势），以及较大的阻挡物。 正面和接近正面的人脸可提供最佳效果

-----

