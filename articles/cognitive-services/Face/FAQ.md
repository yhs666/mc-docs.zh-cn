---
title: 常见问题解答 - 人脸 API
titlesuffix: Azure Cognitive Services
description: 下面是有关人脸 API 服务的最常见问题的解答。
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
origin.date: 01/26/2017
ms.date: 03/27/2019
ms.author: v-junlch
ms.openlocfilehash: 43789cc16cf37f0b144e0195fde8ad3111c211bf
ms.sourcegitcommit: c5599eb7dfe9fd5fe725b82a861c97605635a73f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2019
ms.locfileid: "58505396"
---
# <a name="face-api-frequently-asked-questions"></a>人脸 API 常见问题解答

> [!TIP]
> 如果在本“常见问题解答”中找不到问题的答案，请尝试在 [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) 中向人脸 API 社区提问，或联系 [UserVoice 上的帮助和支持人员](https://cognitive.uservoice.com/)。

-----
**问**：哪些因素会降低人脸 API 的识别、验证或查找相似这些功能的准确性？

**答**：这通常与肉眼难以识别某人的情况相同，具体包括以下情形：
* 有障碍物挡住了一只或两只眼睛
* 刺眼的光（例如，严重逆光）
* 发型或面部毛发发生变化
* 由于年龄不同而发生变化
* 极端的面部表情（例如，尖叫）

人脸 API 通常能够成功应对此类难题，但准确性可能降低。 若要提高识别的可靠性并解决这些难题，请使用包含各种角度和照明的照片训练“人员”模型。

-----
**问**：我正在传入二进制图像数据，但却看到“人脸图像无效”错误消息。

**答**：此错误意味着算法在分析图像时出现问题。 原因包括：
* 支持的输入图像格式包括 JPEG、PNG、GIF（第一帧）和 BMP。
* 图像文件不得大于 4 MB
* 可检测的面部大小范围为 36x36 到 4096x4096 像素。 无法检测超出此范围的人脸
* 某些人脸可能因技术难题而无法检测，例如非常大的人脸角度（头部姿势），以及较大的阻挡物。 正面和接近正面的人脸可提供最佳效果

-----

<!-- Update_Description: wording update -->