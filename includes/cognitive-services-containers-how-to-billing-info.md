---
author: diberry
ms.author: v-junlch
ms.service: cognitive-services
ms.topic: include
origin.date: 04/16/2019
ms.date: 05/16/2019
ms.openlocfilehash: c7b4b602243ccea7971f14526897f33040387841
ms.sourcegitcommit: 2312c8153c559ed1a235d029c7522283d9c92864
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/16/2019
ms.locfileid: "65730994"
---
对该容器的查询在用于 `<ApiKey>` 的 Azure 资源的定价层计费。

如果未连接到计费终结点进行计量，则认知服务容器不会被许可运行。 客户需始终让容器可以向计费终结点传送计费信息。 认知服务容器不会将客户数据（例如，正在分析的图像或文本）发送给 Microsoft。 

### <a name="connecting-to-azure"></a>连接到 Azure

容器需要计费参数值才能运行。 这些值使容器可以连接到计费终结点。 容器约每 10 到 15 分钟报告一次使用情况。 如果容器未在允许的时间范围内连接到 Azure，容器会继续运行，但不会为查询提供服务，直到计费终结点还原。 尝试连接按 10 到 15 分钟的相同时间间隔进行 10 次。 如果无法在 10 次尝试内连接到计费终结点，容器会停止运行。 

### <a name="billing-arguments"></a>计费参数

必须使用有效值指定所有以下三个选项，才能使 `docker run` 命令启动容器：

| 选项 | 说明 |
|--------|-------------|
| `ApiKey` | 用于跟踪账单信息的认知服务资源的 API 密钥。<br/>必须将此选项的值设置为 `Billing` 中指定的已预配资源的 API 密钥。 |
| `Billing` | 用于跟踪账单信息的认知服务资源的终结点。<br/>必须将此选项的值设置为已预配的 Azure 资源的终结点 URI。|
| `Eula` | 表示已接受容器的许可条款。<br/>此选项的值必须设置为 `accept`。 |



