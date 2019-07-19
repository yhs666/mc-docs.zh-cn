---
title: 容器支持
titleSuffix: Azure Cognitive Services
description: 了解如何验证情绪分析容器实例。
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
origin.date: 06/26/2019
ms.date: 07/09/2019
ms.author: v-junlch
ms.openlocfilehash: 02a238acfdc04a96de86de19ec52adf87f05e2a6
ms.sourcegitcommit: 8f49da0084910bc97e4590fc1a8fe48dd4028e34
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/12/2019
ms.locfileid: "67844998"
---
## <a name="verify-the-sentiment-analysis-container-instance"></a>验证情绪分析容器实例

1. 选择“概述”  选项卡，并复制 IP 地址。
1. 打开新的浏览器选项卡，并使用 IP 地址（例如 `http://<IP-address>:5000 (http://55.55.55.55:5000`）。 此时将显示容器的主页，告知该容器处于运行状态。

    ![查看“容器”主页，以验证它是否处于运行状态](../media/how-tos/container-instance/swagger-docs-on-container.png)

1. 选择“服务 API 说明”  链接，以导航到容器 Swagger 页。

1. 选择任意一个 **POST** API，然后选择“试用”  。此时将显示参数，其中包括示例输入：

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

1. 将该输入替换为以下 JSON：

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

现在，我们便可将响应有效负荷 JSON 的文档 `id` 关联到原始请求有效负荷文档 `id`，并可以发现分数超过了 `.98`，这指示情绪非常积极。

