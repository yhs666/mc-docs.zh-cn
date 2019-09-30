---
title: 免费试用语音服务
titleSuffix: Azure Cognitive Services
description: 以简单、经济的方式开始使用语音服务。 可利用 30 天免费试用了解此服务的功能并确定它能否满足应用程序需求。
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 07/05/2019
ms.date: 09/19/2019
ms.author: v-tawe
ms.custom: seodec18
ms.openlocfilehash: 43ad9d45e88446daf230b6bb7d1fcd4f1dab41fb
ms.sourcegitcommit: b328fdef5f35155562f10817af44f2a4e975c3aa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2019
ms.locfileid: "71267060"
---
# <a name="try-speech-services-for-1rmb-trial"></a>试用 1 元人民币试用版的语音服务

以简单、经济的方式开始使用语音服务。

注册 Azure 帐户时附带了 1500 元人民币的服务额度，你可以将其应用于付费语音服务订阅以获得 30 天试用。

最后，语音服务提供适合用于开发应用程序的免费、低容量层。 服务额度到期后，仍可以保留此免费订阅。

<!--no free trial, use Azure account-->
## <a name="new-azure-account"></a>新 Azure 帐户

新 Azure 帐户将获得 1,500 元人民币的服务额度，可用于申请 30 天试用。 可使用此额度深入探索语音服务或者开始应用程序开发。

若要注册新的 Azure 帐户，请转到 [Azure 注册页](https://www.azure.cn/pricing/1rmb-trial-full/?form-type=identityauth)，使用移动电话号码创建新的 Azure 帐户。

<!--not support Microsoft Account-->

创建 Azure 帐户后，请按照下一部分的步骤开始订阅语音服务。

## <a name="create-a-speech-resource-in-azure"></a>在 Azure 中创建语音资源

若要将语音服务资源（1 元人民币试用或付费层）添加到 Azure 帐户，请执行以下操作：

1. 使用 Azure 帐户登录到 [Azure 门户](https://portal.azure.cn/)。

1. 选择门户左上角的“创建资源”。 

    ![创建资源](media/index/try-speech-api-create-speech1.png)

1. 在“新建”窗口中，搜索“语音”。  

1. 在搜索结果中，选择“语音”。 

    ![选择语音](media/index/try-speech-api-create-speech2.png)

1. 在“语音”下，选择“创建”   按钮。

    ![选择“创建”按钮](media/index/try-speech-api-create-speech3.png)

1. 在“创建”下输入： 

   * 新资源的名称。 名称有助于区分同一服务的多个订阅。
   * 选择新资源关联的 Azure 订阅，以确定计费方式。
   * 选择将使用资源的[区域](regions.md)。
   * 选择免费或付费定价层。 选择“查看全部定价详细信息”，获取每个层的定价和用量配额的完整信息  。
   * 为此“语音”订阅创建新的资源组或将订阅分配到现有资源组。 资源组有助于使多种 Azure 订阅保持有序状态。
   * 为了以后可便捷访问订阅，请选中“固定到仪表板”复选框  。
   * 选择“创建”  。

     ![选择“创建”按钮](media/index/try-speech-api-create-speech4.png)

     创建和部署新语音资源需要一些时间。 选择“快速入门”，查看新资源的信息。 

     ![“快速启动”面板](media/index/try-speech-api-create-speech5.png)

1. 在“快速入门”下，选择步骤 1 中的“密钥”链接，以显示订阅密钥   。 每个订阅有两个密钥；可在应用程序中使用任意一个密钥。 选择每个密钥旁的按钮，可将其复制到剪贴板以粘贴到代码中。

> [!NOTE]
> 可在一个或多个区域中创建数量不受限的标准层订阅。 但是，只能创建一个免费层订阅。 在免费层上进行的模型部署如果连续 7 天处于未使用状态，则会被系统自动停用。

## <a name="switch-to-a-new-subscription"></a>切换到新订阅

若要从一个订阅切换到另一个订阅（例如，在 1 元人民币试用到期时或发布应用程序时），请将代码中的区域和订阅密钥替换为新 Azure 资源的区域和订阅密钥。


* 如果应用程序使用[语音 SDK](speech-sdk.md)，请在创建语音配置时提供区域代码，例如 `chinaeast`。
* 如果应用程序使用某个语音服务的 [REST API](overview.md)，则区域是你在发出请求时使用的终结点 URI 的一部分。

为某个区域创建的密钥仅在该区域有效。 尝试在其他区域使用此类密钥会导致身份验证错误。

## <a name="next-steps"></a>后续步骤

完成我们的 10 分钟快速入门之一，或查看我们的 SDK 示例：

> [!div class="nextstepaction"]
> [语音 SDK 示例](speech-sdk.md#get-the-samples)
