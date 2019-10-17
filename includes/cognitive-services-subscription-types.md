---
author: aahill
ms.service: cognitive-services
ms.topic: include
origin.date: 06/24/2019
ms.date: 10/11/2019
ms.author: v-tawe
ms.openlocfilehash: ac88b15400e1a01b517cd12c780a770cafda6fca
ms.sourcegitcommit: aea45739ba114a6b069f782074a70e5dded8a490
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2019
ms.locfileid: "72275543"
---
<!-- > [!NOTE]
> Subscription owners can disable the creation of Cognitive Services resources for resource groups and subscriptions by applying [Azure policy](/governance/policy/overview#policy-definition), assigning a “Not allowed resource types” policy definition, and specifying **Microsoft.CognitiveServices/accounts** as the target resource type. -->
可以通过两种不同的资源访问 Azure 认知服务：多服务资源或单服务资源。

* 多服务资源：
    * 使用单个密钥和终结点访问多个 Azure 认知服务。
    * 合并来自你使用的服务的计费。
* 单服务资源：
    * 对于创建的每个服务，使用唯一密钥和终结点访问单个 Azure 认知服务。 
    * 使用试用层来试用服务。   
