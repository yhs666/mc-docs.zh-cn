---
title: 使用光学字符识别 (OCR) 提取文本 - 计算机视觉
titleSuffix: Azure Cognitive Services
description: 与使用计算机视觉 API 通过光学字符识别 (OCR) 提取文本相关的概念。
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
origin.date: 02/11/2019
ms.date: 02/27/2019
ms.author: v-junlch
ms.custom: seodec18
ms.openlocfilehash: fa8f1804387420ca253086238c98048340069a98
ms.sourcegitcommit: ea33f8dbf7f9e6ac90d328dcd8fb796241f23ff7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/01/2019
ms.locfileid: "57204052"
---
# <a name="extract-text-with-optical-character-recognition"></a>使用光学字符识别提取文本

计算机视觉的光学字符识别 (OCR) 功能可检测图像中的文本内容，并将已识别的文本转换为计算机可读的字符流。 你可以将结果用于多个目的，例如搜索、医疗记录、安全和银行。 

OCR 支持 25 种语言：阿拉伯语、简体中文、繁体中文、捷克语、丹麦语、荷兰语、英语、芬兰语、法语、德语、希腊语、匈牙利语、意大利语、日语、韩语、挪威语、波兰语、葡萄牙语、罗马尼亚语、俄语、塞尔维亚语(西里尔文和拉丁语)、斯洛伐克语、西班牙语、瑞典语和土耳其语。 OCR 会自动检测所检测到的文本的语言。

如有必要，OCR 会更正已识别文本的旋转，方法是以度为单位返回有关水平图像轴的旋转偏移量。 OCR 还提供每个字词的帧坐标，如下图所示。

![一个示意图，描绘正在旋转的图像及其正在读取和画出的文本](./Images/vision-overview-ocr.png)

## <a name="image-requirements"></a>映像要求

计算机视觉可以使用 OCR 从符合以下要求的图像中提取文本：

- 图像必须以 JPEG、PNG、GIF 或 BMP 格式显示
- 输入图像的大小必须介于 50 x 50 和 4200 x 4200 像素之间
- 图像中的文本可以旋转 90 度的任何倍数再加最多 40 度的小角度。

## <a name="improving-ocr-accuracy"></a>提高 OCR 准确性

文本识别的准确性取决于图像质量。 以下情况可能会导致读数不准确：

- 图像模糊。
- 文本为手书或草书。
- 字体为艺术体。
- 文本大小过小。
- 背景复杂、阴影、文本反光或透视扭曲。
- 文本过长或单词开头缺少大写字母
- 文本包含下标、上标或删除线。

### <a name="ocr-limitations"></a>OCR 限制

在以文本为主的图像上，误报可能来自部分识别的字词。 在某些图像上，尤其是在不带任何文本的照片上，精度会因图像类型而存在较大差异。

## <a name="next-steps"></a>后续步骤

若要了解详细信息，请参阅 [OCR 参考文档](https://dev.cognitive.azure.cn/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fc)。

<!-- Update_Description: wording update -->