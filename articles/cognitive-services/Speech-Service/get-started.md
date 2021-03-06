---
title: 试用语音服务
titleSuffix: Azure Cognitive Services
description: 以简单、经济的方式开始使用语音服务。 可利用 30 天试用了解此服务的功能并确定它能否满足应用程序需求。
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 07/05/2019
ms.date: 09/19/2019
ms.author: v-tawe
ms.custom: seodec18, seo-javascript-october2019
ms.openlocfilehash: b0d0605f021a1c0a6bee827a575a151702eb1cdd
ms.sourcegitcommit: 9e92bcf6aa02fc9e7b3a29abadf6b6d1a8ece8c4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74389385"
---
# <a name="try-speech-services-for-1rmb-trial"></a>试用 1 元人民币试用版的语音服务

以简单、经济的方式开始使用语音服务。

注册 Azure 帐户时附带了 1,500 元人民币的服务额度，你可以将其应用于付费语音服务订阅以获得最多 30 天试用。

<!-- Finally, the Speech Services offers a free, low-volume tier that's suitable for developing applications. You can keep this free subscription even after your service credit expires. -->

<!--no free trial, use Azure account-->
## <a name="new-azure-account"></a>新 Azure 帐户

新 Azure 帐户将收到使用时间长达 30 天的 1,500 元人民币的服务额度。 可使用此额度深入探索语音服务或者开始应用程序开发。

若要注册新的 Azure 帐户，请转到 [Azure 注册页](https://www.azure.cn/pricing/1rmb-trial-full/?form-type=identityauth)，使用移动电话号码创建新的 Azure 帐户。

<!--not support Microsoft Account-->

创建 Azure 帐户后，请按照下一部分的步骤开始订阅语音服务。

## <a name="create-a-speech-resource-in-azure"></a>在 Azure 中创建语音资源

若要将语音服务资源（1 元人民币试用或付费层）添加到 Azure 帐户，请执行以下操作：

1. 使用 Azure 帐户登录到 [Azure 门户](https://portal.azure.cn/)。

1. 选择门户左上角的“创建资源”。 

    ![语音 API - 创建资源](media/index/speech-api-create-resource.png)

1. 在“新建”窗口中，搜索“语音”。  

1. 在搜索结果中，选择“语音”。 

    ![语音 API - 选择语音](media/index/speech-api-select-speech.png)

1. 在“语音”下，选择“创建”   按钮。

    ![语音 API -“创建”按钮](media/index/speech-api-create-button.png)

1. 在“创建”下输入： 

   * 新资源的名称。 名称有助于区分同一服务的多个订阅。
   * 选择新资源关联的 Azure 订阅，以确定计费方式。
   * 选择将使用资源的[区域](regions.md)。
   * 选择免费或付费定价层。 选择“查看全部定价详细信息”，获取每个层的定价和用量配额的完整信息  。
   * 为此“语音”订阅创建新的资源组或将订阅分配到现有资源组。 资源组有助于使多种 Azure 订阅保持有序状态。
   * 为了以后可便捷访问订阅，请选中“固定到仪表板”复选框  。
   * 选择“创建”。 

     ![语音 API - 选择“创建”](media/index/speech-api-select-create.png)

     创建和部署新语音资源需要一些时间。 选择“快速入门”，查看新资源的信息。 

     ![语音 API - 部署资源](media/index/speech-api-deploy-resource.png)

1. 在“快速入门”下，选择步骤 1 中的“密钥”链接，以显示订阅密钥   。 每个订阅有两个密钥；可在应用程序中使用任意一个密钥。 选择每个密钥旁的按钮，可将其复制到剪贴板以粘贴到代码中。

> [!NOTE]
> 可在一个或多个区域中创建数量不受限的标准层订阅。 但是，只能创建一个免费层订阅。 在免费层上进行的模型部署如果连续 7 天处于未使用状态，则会被系统自动停用。

## <a name="switch-to-a-new-subscription"></a>切换到新订阅

若要从一个订阅切换到另一个订阅（例如，在 1 元人民币试用到期时或发布应用程序时），请将代码中的区域和订阅密钥替换为新 Azure 资源的区域和订阅密钥。


* 如果应用程序使用[语音 SDK](speech-sdk.md)，请在创建语音配置时提供区域代码，例如 `chinaeast`。
* 如果应用程序使用某个语音服务的 [REST API](rest-apis.md)，则区域是你在发出请求时使用的终结点 URI 的一部分。

为某个区域创建的密钥仅在该区域有效。 尝试在其他区域使用此类密钥会导致身份验证错误。

## <a name="next-steps"></a>后续步骤

完成我们的 10 分钟快速入门之一，或查看我们的 SDK 示例：

> [!div class="nextstepaction"]
> [快速入门：在 C# 中识别语音](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnet)
> [语音 SDK 示例](speech-sdk.md#get-the-samples)
