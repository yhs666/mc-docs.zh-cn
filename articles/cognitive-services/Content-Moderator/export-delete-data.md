---
title: 导出或删除数据 - 内容审查器
titlesuffix: Azure Cognitive Services
description: 了解如何在内容审查器中导出或删除数据。
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
origin.date: 02/07/2019
ms.date: 03/01/2019
ms.author: v-junlch
ms.openlocfilehash: e57b6a0378e00e2f047a28d3f77b8cffebfa53ea
ms.sourcegitcommit: ea33f8dbf7f9e6ac90d328dcd8fb796241f23ff7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/01/2019
ms.locfileid: "57203983"
---
# <a name="export-or-delete-user-data-in-content-moderator"></a>在内容审查器中导出或删除用数据

内容审查器收集用户数据来使服务运转，但客户有完全控制权限使用[审核与评审 API](/cognitive-services/content-moderator/api-reference) 来查看、导出及删除其数据。

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

有关如何在内容审查器中导出和删除用户数据的详细信息，请参阅下表。

| 数据 | 导出操作 | 删除操作 |
| ---- | ---------------- | ---------------- |
| 帐户信息（订阅密钥） | 不适用 | 使用 Azure 门户（Azure 订阅）进行删除。|
| 用于自定义匹配的图像 | 调用[获取图像 ID API](https://dev.cognitive.azure.cn/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f676)。 映像以单向专有哈希格式存储，并且无法提取实际映像。 | 调用[删除所有图像 API](https://dev.cognitive.azure.cn/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f686)。 或者使用 Azure 门户删除内容审查器资源。 |
| 用于自定义匹配的术语 | 调用[获取所有词条 API](https://dev.cognitive.azure.cn/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67e) | 调用[删除所有词条 API](https://dev.cognitive.azure.cn/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67d)。 或者使用 Azure 门户删除内容审查器资源。 |
| 审阅 | 调用[获取评审 API](https://dev.cognitive.azure.cn/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2) |



<!-- Update_Description: wording update -->
