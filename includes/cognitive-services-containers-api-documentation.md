---
author: diberry
ms.author: v-junlch
ms.service: cognitive-services
ms.topic: include
origin.date: 03/25/2019
ms.date: 06/11/2019
ms.openlocfilehash: 2a401c2c7153390cc2b4d9b0927d9578b03ff894
ms.sourcegitcommit: 259c97c9322da7add9de9f955eac275d743c9424
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/11/2019
ms.locfileid: "66830150"
---
## <a name="validate-that-a-container-is-running"></a>验证容器是否正在运行 

有几种方法可用于验证容器是否正在运行。 

|请求|目的|
|--|--|
|`http://localhost:5000/`|容器提供主页。|
|`http://localhost:5000/status`|使用 GET 进行了请求，从而在不会导致终结点查询的情况下验证容器是否正在运行。 此请求可用于 Kubernetes [运行情况和就绪情况探测](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/)。|
|`http://localhost:5000/swagger`|容器提供终结点以及 `Try it now` 功能的整套文档。 使用此功能可以将设置输入到基于 Web 的 HTML 表单并进行查询，而无需编写任何代码。 查询返回后，将提供示例 CURL 命令，用于演示所需的 HTTP 标头和正文格式。 |

![容器的主页](./media/cognitive-services-containers-api-documentation/container-webpage.png)

