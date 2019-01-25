---
title: 导出或删除数据 - 内容审查器
titlesuffix: Azure Cognitive Services
description: 了解如何在内容审查器中导出或删除数据。
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
origin.date: 05/25/2018
ms.date: 01/22/2019
ms.author: v-junlch
ms.openlocfilehash: 0028a3e8726a38800a81df36c626ffcab69f9ec7
ms.sourcegitcommit: f248afb1039011d34579baed2980f0632061f5b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2019
ms.locfileid: "54858078"
---
# <a name="export-or-delete-user-data-in-content-moderator"></a>在内容审查器中导出或删除用数据

内容审查器收集用户数据来使服务运转，但客户拥有使用 [API](/cognitive-services/content-moderator/api-reference) 来查看、导出和删除数据的完全控制权限。

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

有关如何在内容审查器中导出和删除用户数据的详细信息，请参阅下表。

| 数据 | 导出操作 | 删除操作 |
| ---- | ---------------- | ---------------- |
| 帐户信息（订阅密钥） | 不适用 | 使用 Azure 门户（Azure 订阅）进行删除。|
| 用于自定义匹配的图像 | [获取图像 ID](https://dev.cognitive.azure.cn/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f676)。 映像以单向专有哈希格式存储，并且无法提取实际映像。 | [删除所有图像](https://dev.cognitive.azure.cn/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f686)。 或者使用 Azure 门户删除内容审查器资源。 |
| 用于自定义匹配的术语 | [获取所有术语](https://dev.cognitive.azure.cn/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67e) | [删除所有术语](https://dev.cognitive.azure.cn/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67d) 或者使用 Azure 门户删除内容审查器资源。 |
| 审阅 | [获取评审](https://dev.cognitive.azure.cn/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2) | 



