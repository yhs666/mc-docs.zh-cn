---
title: 描述成人和不雅内容 - 计算机视觉
titleSuffix: Azure Cognitive Services
description: 使用计算机视觉 API 检测图像中的成人和不雅内容的相关概念。
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
origin.date: 08/29/2018
ms.date: 10/30/2018
ms.author: v-junlch
ms.openlocfilehash: fec0005ad7b5f2013ea2792cb4730463ad7f4295
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52666756"
---
# <a name="detecting-adult-and-racy-content"></a>检测成人和不雅内容

视觉类别包含成人组和种族组，其中的成人组用于检测成人内容，限制显示包含色情内容的图像。 可以根据用户的首选项，相应地设置进行成人和种族内容检测的筛选器。

## <a name="defining-adult-and-racy-content"></a>定义成人和不雅内容

在[分析图像方法](https://dev.cognitive.azure.cn/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fa)所涵盖的各种视觉特征中，成人视觉特征可以检测成人和不雅图像。 “成人”图像定义为实质上淫秽并且通常描绘裸体和性行为的图像。 “不雅”图像定义为实际具有性暗示且往往包含一些性露骨内容（比“成人”图像少）的图像。 成人视觉特征类型通常用于限制包含性暗示和明确性内容的图像的显示。

## <a name="identifying-adult-and-racy-content"></a>标识成人和不雅内容

分析图像方法在方法的 JSON 响应中返回两个属性（`isAdultContent` 和 `isRacyContent`），它们分别表示成人和不雅内容。 这两个属性都返回一个布尔值（true 或 false）。 该方法还返回两个属性 `adultScore` 和 `racyScore`，它们分别表示用于识别成人和不雅内容的置信度分数。 可以在滑尺上设置成人和不雅内容检测的置信度筛选器，从而根据具体情况调整偏好。

## <a name="next-steps"></a>后续步骤

了解关于[检测特定于域的内容](concept-detecting-domain-content.md)和[检测人脸](concept-detecting-faces.md)的概念。

