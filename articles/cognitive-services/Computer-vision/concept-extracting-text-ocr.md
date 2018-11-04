---
title: 使用 OCR 提取文本 - 计算机视觉
titleSuffix: Azure Cognitive Services
description: 与使用计算机视觉 API 通过光学字符识别 (OCR) 提取文本相关的概念。
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
origin.date: 08/29/2018
ms.date: 10/30/2018
ms.author: v-junlch
ms.openlocfilehash: 6b1a12db62b5d6f8634b4722698b998683a6b48f
ms.sourcegitcommit: b8e99939a5493a15b78c32e87bfbf76a8c96a84a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "50409041"
---
# <a name="extracting-text-with-ocr"></a>使用 OCR 提取文本

计算机视觉中的光学字符识别 (OCR) 技术可检测图像中的文本内容，并将已识别的文本提取到计算机可读的字符流中。 可以将结果用于搜索和各种其他目的，例如医疗记录、安全性、银行操作。 它会自动检测语言。 OCR 可以节省用户的时间，为用户带来方便，因为用户只需对文本拍照即可，不需进行文本转录。

OCR 支持 25 种语言。 这些语言包括：阿拉伯语、简体中文、繁体中文、捷克语、丹麦语、荷兰语、英语、芬兰语、法语、德语、希腊语、匈牙利语、意大利语、日语、韩语、挪威语、波兰语、葡萄牙语、罗马尼亚语、俄语、塞尔维亚语（西里尔文和拉丁语）、斯洛伐克语、西班牙语、瑞典语、土耳其语。

OCR 可以根据需要，围绕图像的水平轴将识别的文本旋转相应的度数，对倾斜的文本进行纠正。 OCR 提供每个字词的帧坐标，如下图所示。

![OCR 概述](./Images/vision-overview-ocr.png)

## <a name="ocr-requirements"></a>OCR 要求

计算机视觉可以使用 OCR 从符合以下要求的图像中提取文本：

- 图像必须以 JPEG、PNG、GIF 或 BMP 格式显示
- 输入图像的大小必须介于 50 x 50 和 4200 x 4200 像素之间


输入图像可以旋转 90 度的任何倍数再加最多 40 度的小角度。

## <a name="improving-ocr-accuracy"></a>提高 OCR 准确性

文本识别的准确性取决于图像质量。 以下情况可能导致读取不准确：

- 图像模糊。
- 文本为手书或草书。
- 字体为艺术体。
- 文本大小过小。
- 背景复杂、阴影、文本反光或透视扭曲。
- 文本过长或单词开头缺少大写字母
- 文本包含下标、上标或删除线。

### <a name="ocr-limitations"></a>OCR 限制

在以文本为主的照片上，误报可能来自部分识别的字词。 在某些照片上，尤其在不带任何文本的照片上，精度会因图像类型而存在较大差异。

## <a name="next-steps"></a>后续步骤

了解有关[识别打印文本和手写文本](concept-recognizing-text.md)的概念。

