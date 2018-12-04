---
title: API 参考 - 人脸 API
titleSuffix: Azure Cognitive Services
description: 此 API 参考提供了有关人员管理、LargePersonGroup/PersonGroup 管理、LargeFaceList/FaceList 管理和人脸算法 API 的信息。
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: reference
origin.date: 03/01/2018
ms.date: 11/26/2018
ms.author: v-junlch
ms.openlocfilehash: d4ca083e4bf806da175541e20fe069c2e5099aaf
ms.sourcegitcommit: bfd0b25b0c51050e51531fedb4fca8c023b1bf5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52672776"
---
# <a name="api-reference"></a>API 参考

Microsoft 人脸 API 是基于云的 API，为人脸检测和识别提供先进的算法。

人脸 API 涵盖了以下类别：

- [人脸算法 API](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)：涵盖了诸如[检测](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)、[查找相似](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237)、[验证](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a)、[识别](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)和[分组](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238)之类的核心功能。
- [FaceList 管理 API](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b)：用来管理用于[查找相似](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237)的 FaceList。
- [LargePersonGroup 人员管理 API](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40)：用来管理用于[识别](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)的 LargePersonGroup 人脸。
- [LargePersonGroup 管理 API](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)：用来管理用于[识别](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)的 LargePersonGroup 数据集。
- [LargeFaceList 管理 API](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc)：用来管理用于[查找相似](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237)的 LargeFaceList。
- [PersonGroup Person 管理 API](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)：用来管理用于[识别](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)的 PersonGroup 人脸。
- [PersonGroup 管理 API](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)：用来管理用于[识别](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)的 PersonGroup 数据集。

<!-- Linguist question: Please confirm that the following are API names and should be left as is: "Person Management, LargePersonGroup/PersonGroup Management, LargeFaceList/FaceList Management, and Face Algorithms" -->

<!-- Update_Description: wording update -->