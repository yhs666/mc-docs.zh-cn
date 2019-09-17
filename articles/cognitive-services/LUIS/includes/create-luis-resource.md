---
title: 容器支持
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: 4d6b545ab79c518fc109fd08511e924370fecbee
ms.sourcegitcommit: 13642a99cc524a416b40635f48676bbf5cdcdf3d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2019
ms.locfileid: "70103859"
---
## <a name="create-a-luis-resource"></a>创建 LUIS 资源

1. 登录到 [Azure 门户](https://portal.azure.cn)
1. 单击“[创建**语言理解**](https://ms.portal.azure.cn/#create/Microsoft.CognitiveServicesLUIS)”
1. 输入所有必需的设置：

    |设置|值|
    |--|--|
    |Name|所需名称（2-64 个字符）|
    |订阅|选择相应的订阅|
    |Location|选择附近任何可用的位置|
    |定价层|`F0` - 最小定价层|
    |资源组|选择可用的资源组|

1. 单击“创建”  并等待创建资源。 创建资源后，导航到“资源”页
1. 收集配置的 `endpoint` 和 API 密钥：

    |门户中的“资源”选项卡|设置|Value|
    |--|--|--|
    |**概述**|终结点|复制终结点。 它看起来类似于 `https://luis.cognitiveservices.azure.cn/luis/v2.0`|
    |密钥 |API 密钥|复制两个密钥中的 1 个。 它是一个由 32 个字母数字组成的字符串（不包含空格或短划线），即 `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`。|
