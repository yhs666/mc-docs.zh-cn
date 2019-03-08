---
title: 注册文本分析 API
titleSuffix: Azure Cognitive Services
description: 有关注册和使用文本分析服务的说明。
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
origin.date: 02/13/2019
ms.date: 03/01/2019
ms.author: v-junlch
ms.openlocfilehash: 5eefdbfb8952f3b80e42c2a00247b0e80645b6c8
ms.sourcegitcommit: ea33f8dbf7f9e6ac90d328dcd8fb796241f23ff7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/01/2019
ms.locfileid: "57204196"
---
# <a name="how-to-sign-up-for-the-text-analytics-api"></a>如何注册文本分析 API

文本分析资源在云中全天候提供。 在上传需分析的内容之前，必须通过注册获取访问密钥。 每次调用 API 都需要提供请求的访问密钥。

+ 从 Azure 订阅着手。 可以创建 [1 元试用帐户](https://www.azure.cn/zh-cn/pricing/1rmb-trial-full/?form-type=identityauth)进行试验。

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

<!-- Update_Description: update metedata properties -->
