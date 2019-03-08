---
author: diberry
ms.author: v-junlch
ms.service: cognitive-services
ms.topic: include
origin.date: 002/08/2019
ms.date: 03/01/2019
ms.openlocfilehash: 09b36ed72fc67d03aacb77c18a2e74294b3a37bf
ms.sourcegitcommit: ea33f8dbf7f9e6ac90d328dcd8fb796241f23ff7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/01/2019
ms.locfileid: "57204207"
---
如果未连接到 Azure 进行计量，则无法授权并运行认知服务容器。 客户需要始终让容器向计量服务传送账单信息。 认知服务容器不会将客户数据（例如，正在分析的图像或文本）发送给 Microsoft。 容器约每 10 到 15 分钟报告一次使用情况。

`docker run` 使用以下参数以便计费：

| 选项 | 说明 |
|--------|-------------|
| `ApiKey` | 用于跟踪账单信息的认知服务资源的 API 密钥。<br/>必须将此选项的值设置为 `Billing` 中指定的已预配资源的 API 密钥。 |
| `Billing` | 用于跟踪账单信息的认知服务资源的终结点。<br/>必须将此选项的值设置为已预配的 LUIS Azure 资源的终结点 URI。|
| `Eula` | 表示已接受容器的许可条款。<br/>此选项的值必须设置为 `accept`。 |

> [!IMPORTANT]
> 必须使用有效值指定所有三个选项，否则容器将无法启动。

<!-- ms.date: 03/01/2019 -->