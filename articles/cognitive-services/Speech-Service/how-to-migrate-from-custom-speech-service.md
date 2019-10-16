---
title: 从自定义语音服务迁移到语音服务
titleSuffix: Azure Cognitive Services
description: 自定义语音服务现在是语音服务的一部分。 切换到语音服务可受益于最新的质量和功能更新。
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 10/01/2018
ms.date: 09/23/2019
ms.author: v-tawe
ms.custom: seodec18
ms.openlocfilehash: a2e6c9f2c3bc5437ccfc174fd98c70c4892f1001
ms.sourcegitcommit: b328fdef5f35155562f10817af44f2a4e975c3aa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2019
ms.locfileid: "71267054"
---
# <a name="migrate-from-the-custom-speech-service-to-the-speech-services"></a>从自定义语音服务迁移到语音服务

本文介绍如何将应用程序从自定义语音服务迁移到语音服务。

自定义语音服务现在是语音服务的一部分。 切换到语音服务可受益于最新的质量和功能更新。

## <a name="migration-for-new-customers"></a>新客户的迁移

使用语音服务的按小时定价模型更简单。  

1. 在应用程序的每个可用区域中创建一个 Azure 资源。 Azure 资源名称为 **Speech**。 可对同一区域中的以下服务使用单个 Azure 资源，而无需创建不同的资源：

    * 文本转语音

2. 下载[语音 SDK](speech-sdk.md)。

3. 遵照快速入门指南和 SDK 示例使用正确的 API。 如果使用 REST API，则还需要使用正确的终结点和资源密钥。

4. 将客户端应用程序更新为使用语音服务和 API。

## <a name="migration-for-existing-customers"></a>现有客户的迁移

将现有资源密钥迁移到语音服务门户上的语音服务。 使用以下步骤：

> [!NOTE]
> 只能迁移同一区域中的资源密钥。

1. 登录到 [cris.ai](https://cris.ai/Home/CustomSpeech) 门户，然后在右上角菜单中选择订阅。

2. 选择“迁移选定的订阅”。 

3. 在文本框中输入订阅密钥，然后选择“迁移”  。

## <a name="next-steps"></a>后续步骤

* [试用语音服务](get-started.md)。

## <a name="see-also"></a>另请参阅

* [什么是语音服务](overview.md)
* [语音服务和语音 SDK 文档](speech-sdk.md#get-the-sdk)
