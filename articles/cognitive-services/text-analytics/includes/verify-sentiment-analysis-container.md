---
title: 验证情绪分析容器实例
titleSuffix: Azure Cognitive Services
description: 了解如何验证情绪分析容器实例。
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
origin.date: 06/26/2019
ms.date: 07/24/2019
ms.author: v-junlch
ms.openlocfilehash: f1c1a973897b6291ea13e46fb6c189c310ed81b5
ms.sourcegitcommit: 9a330fa5ee7445b98e4e157997e592a0d0f63f4c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2019
ms.locfileid: "68440254"
---
## <a name="verify-the-sentiment-analysis-container-instance"></a>验证情绪分析容器实例

1. 选择“概述”  选项卡，并复制 IP 地址。
1. 打开新的浏览器选项卡，并输入 IP 地址。 例如，输入 `http://<IP-address>:5000 (http://55.55.55.55:5000`)。 此时将显示容器的主页，告知该容器处于运行状态。

    ![查看容器主页，以验证它是否处于运行状态](../media/how-tos/container-instance/swagger-docs-on-container.png)上获取。

1. 选择“服务 API 说明”  链接，以转到容器 swagger 页。

1. 选择任意一个 **POST** API，然后选择“试用”  。此时将显示参数，其中包括此示例输入：

    ```json
    {
      "documents": [
        {
          "language": "en",
          "id": "1",
          "text": "Hello world. This is some input text that I love."
        },
        {
          "language": "fr",
          "id": "2",
          "text": "Bonjour tout le monde"
        },
        {
          "language": "es",
          "id": "3",
          "text": "La carretera estaba atascada. Había mucho tráfico el día de ayer."
        }
      ]
    }
    ```

1. 将该输入替换为以下 JSON 内容：

    ```json
    {
      "documents": [
        {
          "language": "en",
          "id": "7",
          "text": "I was fortunate to attend the KubeCon Conference in Barcelona, it is one of the best conferences I have ever attended. Great people, great sessions and I thoroughly enjoyed it!"
        }
      ]
    }
    ```

1. 将“showStats”  设置为 true。

1. 选择“执行”  以确定文本的情绪。

    在容器中打包的模型会生成范围从 0 到 1 的分数，其中，0 表示消极，而 1 表示积极。

    返回的 JSON 响应包括更新的文本输入的情绪：

    ```json
    {
      "documents": [
      {
        "id": "7",
        "score": 0.9826303720474243,
        "statistics": {
          "charactersCount": 176,
            "transactionsCount": 1
          }
        }
      ],
      "errors": [],
      "statistics": {
        "documentsCount": 1,
        "validDocumentsCount": 1,
        "erroneousDocumentsCount": 0,
        "transactionsCount": 1
      }
    }
    ```

我们现在可以将响应有效负载的 JSON 数据的文档 `id` 关联到原始请求有效负载文档 `id`。 我们会看到分数超过 `.98`，表示非常积极的情绪。

<!-- Update_Description: wording update -->