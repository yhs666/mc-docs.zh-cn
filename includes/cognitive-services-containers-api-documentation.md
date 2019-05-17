---
author: diberry
ms.author: v-junlch
ms.service: cognitive-services
ms.topic: include
origin.date: 03/25/2019
ms.date: 04/23/2019
ms.openlocfilehash: 11d4a112bd7e062b543b3577807e2cbbbcb39c8f
ms.sourcegitcommit: 9642fa6b5991ee593a326b0e5c4f4f4910f50742
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2019
ms.locfileid: "64855698"
---
## <a name="validate-container-is-running"></a>验证容器是否正在运行 

可以通过多种方式验证容器是否正在运行： 

|请求|目的|
|--|--|
|`http://localhost:5000/`|容器提供主页。|
|`http://localhost:5000/status`|通过 GET 进行请求，以便验证容器是否正在运行，不会导致终结点查询操作。 这可以用于 Kubernetes [活跃度和就绪情况探测](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/)。|
|`http://localhost:5000/swagger`|容器提供终结点以及 `Try it now` 功能的整套文档。 通过此功能可以将设置输入到基于 Web 的 HTML 窗体中并进行查询，而无需编写任何代码。 返回查询后，将提供示例 CURL 命令，用于演示所需的 HTTP 标头和正文格式。 |

![容器的主页](./media/cognitive-services-containers-api-documentation/container-webpage.png)

