---
title: 容器存储库和映像
services: cognitive-services
author: IEvangelist
manager: nitinme
description: 表示所有认知服务产品的容器注册表、存储库和映像名称的两个表。
ms.service: cognitive-services
ms.topic: include
origin.date: 09/06/2019
ms.date: 10/11/2019
ms.author: v-tawe
ms.openlocfilehash: bedcbeb354adf2cdce3a9d4e125d257dc37ebc59
ms.sourcegitcommit: aea45739ba114a6b069f782074a70e5dded8a490
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2019
ms.locfileid: "72275541"
---
### <a name="container-repositories-and-images"></a>容器存储库和映像

下表是 Azure 认知服务提供的可用容器映像的综合列表。

#### <a name="public-ungated-container-registry-mcrmicrosoftcom"></a>公共“非封闭”（容器注册表：`mcr.microsoft.com`）

Microsoft 容器注册表托管了认知服务的所有公共可用的“非封闭”容器。

| 服务 | 容器 | 容器注册表/存储库/映像名称 |
|--|--|--|
| [LUIS](../../LUIS/luis-container-howto.md) | LUIS | `mcr.microsoft.com/azure-cognitive-services/luis` |
| [文本分析](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | 关键短语提取 | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
| [文本分析](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | 语言检测 | `mcr.microsoft.com/azure-cognitive-services/language` |
| [文本分析](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | 情绪分析 | `mcr.microsoft.com/azure-cognitive-services/sentiment` |

#### <a name="public-gated-preview-container-registry-containerpreviewazurecrcn"></a>公共“门控”预览版（容器注册表：`containerpreview.azurecr.cn`）

容器预览注册表托管认知服务的所有公共可用“门控”容器。 这些容器需要正式的访问请求才能使用它们。

| 服务 | 容器 | 容器注册表/存储库/映像名称 |
|--|--|--|
| [语音服务 API](../../speech-service/speech-container-howto.md) | 文本转语音 | `containerpreview.azurecr.cn/microsoft/cognitive-services-text-to-speech` |

<!-- | [Computer Vision](../../Computer-vision/computer-vision-how-to-install-containers.md) | Recognize Text | `containerpreview.azurecr.cn/microsoft/cognitive-services-recognize-text` | -->
<!-- | [Face](../../face/face-how-to-install-containers.md) | Face | `containerpreview.azurecr.cn/microsoft/cognitive-services-face` | -->