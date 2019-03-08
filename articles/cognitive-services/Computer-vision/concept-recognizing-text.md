---
title: 识别印刷文本、手写文本 - 计算机视觉
titleSuffix: Azure Cognitive Services
description: 与使用计算机视觉 API 识别图像中的印刷文本和手写文本相关的概念。
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
origin.date: 08/29/2018
ms.date: 02/27/2019
ms.author: v-junlch
ms.custom: seodec18
ms.openlocfilehash: a1b4a1468290af54ee67caeb961cb9892b050bba
ms.sourcegitcommit: ea33f8dbf7f9e6ac90d328dcd8fb796241f23ff7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/01/2019
ms.locfileid: "57204159"
---
# <a name="recognizing-printed-and-handwritten-text"></a>识别打印文本和手写文本

计算机视觉可以在图像中检测并提取印刷文本或手写文本，这些图像包含各种具有不同表面和背景的对象（例如收据、海报、名片、信函、白板）。

文本识别功能与[光学字符识别 (OCR)](concept-extracting-text-ocr.md) 非常相似，但与 OCR 不同，它以异步方式执行并使用更新的识别模型。

> [!NOTE]
> 此技术当前处于预览状态，且仅适用于英语文本。

## <a name="text-recognition-requirements"></a>文本识别要求

计算机视觉可以识别符合以下要求的图像中的印刷和手写文本：

- 图像必须以 JPEG、PNG 或 BMP 格式显示
- 图像的文件大小必须不到 4 兆字节 (MB)
- 图像的尺寸必须介于 50 x 50 和 4200 x 4200 像素之间

## <a name="next-steps"></a>后续步骤

请参阅[识别文本参考文档](https://dev.cognitive.azure.cn/docs/services/56f91f2d778daf23d8ec6739/operations/587f2c6a154055056008f200)了解详细信息。

<!-- Update_Description: wording update -->