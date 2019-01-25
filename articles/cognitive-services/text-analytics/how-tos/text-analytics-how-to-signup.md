---
title: 注册文本分析 API
titleSuffix: Azure Cognitive Services
description: 说明如何在注册后使用文本分析以及如何在受限情况下运行。
services: cognitive-services
author: WenJason
manager: digimobile
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: conceptual
origin.date: 09/12/2018
ms.date: 01/28/2019
ms.author: v-jay
ms.openlocfilehash: f4fd725739ca429a6307132321b732b186d5d972
ms.sourcegitcommit: f248afb1039011d34579baed2980f0632061f5b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2019
ms.locfileid: "54858070"
---
# <a name="how-to-sign-up-for-the-text-analytics-api"></a>如何注册文本分析 API

文本分析资源在云中全天候提供。 在上传需分析的内容之前，必须通过注册获取访问密钥。 每次调用 API 都需要提供请求的访问密钥。

+ 从 Azure 订阅着手。 可以创建 [1 元试用帐户](https://www.azure.cn/zh-cn/pricing/1rmb-trial-full/?form-type=identityauth)进行试验，无需支付任何费用。

+ 创建[认知服务 API 帐户](/cognitive-services/cognitive-services-apis-create-account)，选择“文本分析 API”。 密钥在注册时生成。

就文本分析来说，仅提供“免费”层供用户浏览和评估。

预览版或免费层的服务没有服务级别协议。 有关详细信息，请参阅[认知服务的 SLA](https://www.azure.cn/support/sla/cognitive-services/)

## <a name="how-billing-works"></a>计费原理

有关详细信息，请参阅[定价页](https://www.azure.cn/pricing/details/cognitive-services/)。

### <a name="what-constitutes-a-transaction-in-the-text-analytics-api"></a>文本分析 API 中的事务由什么构成？
文档的任何注释都算作事务。 批处理评分调用也会考虑该事务中需评分的文档数。 因此，举例来说，如果通过一次 API 调用发送 1,000 份文档供情绪分析，系统会将其记为 1,000 个事务。

## <a name="see-also"></a>另请参阅 

 [文本分析概述](../overview.md)  
 [常见问题解答 (FAQ)](../text-analytics-resource-faq.md)

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [获取访问密钥](text-analytics-how-to-access-key.md)
