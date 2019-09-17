---
title: 规划应用
titleSuffix: Language Understanding - Azure Cognitive Services
description: 概述相关应用意向和实体，然后在语言理解智能服务 (LUIS) 中创建应用程序计划。
services: cognitive-services
author: lingliw
manager: digimobile
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: v-lingwu
ms.openlocfilehash: 17b158874f54335007db0ab68b34fc34519d3966
ms.sourcegitcommit: 13642a99cc524a416b40635f48676bbf5cdcdf3d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2019
ms.locfileid: "70104011"
---
# <a name="plan-your-luis-app-with-subject-domain-intents-and-entities"></a>使用主题域、意向和实体规划 LUIS 应用

若要规划应用，请标识主题领域。 这包括与你的应用程序相关的可能意向和实体。  

## <a name="identify-your-domain"></a>标识域

LUIS 应用以特定域的主题为中心。  例如，可能有一个用于预订门票、航班、酒店和租车的旅行应用。 另一应用则用于提供与锻炼、跟踪健身活动和设定目标相关的内容。 标识域可帮助你查找对你的域很重要的单词或短语。

> [!TIP]
> LUIS 提供许多常见场景的预生成域。
> 检查是否可使用预生成域作为应用的起始点。

## <a name="identify-your-intents"></a>标识意向

考虑一下对应用程序任务极为重要的[意向](luis-concept-intent.md)。 以旅行应用为例，该应用可预订航班并检查用户目的地的天气。 可为这些操作定义“BookFlight”和“GetWeather”意向。 对于具有更多功能的复杂应用，意向也就更多，应仔细进行定义，不能过于细致。 例如，“BookFlight”和“BookHotel”可能是单独的意向，但“BookInternationalFlight”和“BookDomesticFlight”则过于相似。

> [!NOTE]
> 最佳做法是仅使用所需意向来执行应用功能。 如果定义的意向过多，LUIS 将难以对话语进行正确分类。 如果定义的太少，则话语可能太过笼统以致于重叠。

## <a name="create-example-utterances-for-each-intent"></a>为每个意向创建示例陈述

确定了意向后，为每个意向创建 15 到 30 个示例话语。 首先，不要少于此数目或者为每个意向创建太多话语。 每个陈述应不同于上一陈述。 良好的陈述样本包括总体字数统计、选词、动词时态和标点。 

有关详细信息，请查看[话语](luis-concept-utterance.md)。

## <a name="identify-your-entities"></a>标识实体

在示例陈述中，标识要提取的实体。 若要预订航班，则需要“目的地”、“日期”、“航空公司”、“机票类别”和“旅行舱位”等信息。 为这些数据类型创建实体，然后在示例话语中标记[实体](luis-concept-entity-types.md)，因为它们对于实现意向很重要。 

确定要在应用中使用哪些实体后，请记住，有不同类型的实体可用于捕获对象类型间的关系。 [LUIS 中的实体](luis-concept-entity-types.md)提供有关不同类型的详细信息。

## <a name="next-steps"></a>后续步骤

应用经过培训、发布并获得终结点陈述后，计划通过[主动学习](luis-how-to-review-endpoint-utterances.md)、[短语列表](luis-concept-feature.md)和[模式](luis-concept-patterns.md)来实现预测改进。 


* 有关如何创建 LUIS 应用的快速演练，请参阅[创建首个语言理解智能服务 (LUIS) 应用](luis-get-started-create-app.md)。




