---
title: 运行 Azure 容器实例
titleSuffix: Azure Cognitive Services
description: 将 LUIS 容器部署到 Azure 容器实例，并在 Web 浏览器中对其进行测试。
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
origin.date: 8/26/2019
ms.date: 9/6/2019
ms.author: v-lingwu
ms.openlocfilehash: be06ed4e07e47030933e584e93649db76995308e
ms.sourcegitcommit: 2f2ced6cfaca64989ad6114a6b5bc76700870c1a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71330434"
---
# <a name="deploy-the-language-understanding-luis-container-to-azure-container-instances"></a>将语言理解 (LUIS) 容器部署到 Azure 容器实例

了解如何部署认知服务 [LUIS](luis-container-howto.md)。 此过程演示如何创建异常探测器资源。 然后讨论如何拉取关联的容器映像。 最后，我们重点介绍了从浏览器中执行这两个业务流程的能力。 使用容器可以将开发人员的注意力从管理基础结构转移到应用程序开发上。

[!INCLUDE [Prerequisites](../containers/includes/container-prerequisites.md)]

[!INCLUDE [Create LUIS resource](includes/create-luis-resource.md)]

[!INCLUDE [Create LUIS Container Instance resource](../containers/includes/create-container-instances-resource.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Next steps](../containers/includes/containers-next-steps.md)]
