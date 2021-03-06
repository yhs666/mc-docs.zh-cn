---
author: ggailey777
ms.service: azure-functions
ms.topic: include
origin.date: 09/26/2019
ms.date: 10/28/2019
ms.author: v-junlch
ms.openlocfilehash: 6c8574e2f1650cbbda8df0946e68c506f59e0431
ms.sourcegitcommit: 7d2ea8a08ee329913015bc5d2f375fc2620578ba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034501"
---
你已在 Azure 中创建了一个函数应用以及所需的存储帐户。 此帐户的连接字符串安全存储在 Azure 中的应用设置内。 在本文中，你要将消息写入到同一帐户中的存储队列。 若要在本地运行函数时连接到该存储帐户，必须将应用设置下载到 local.settings.json 文件。 

从项目的根目录中，运行以下 Azure Functions Core Tools 命令将设置下载到 local.settings.json，并将 `<APP_NAME>` 替换为上一篇文章中函数应用的名称：

```bash
func azure functionapp fetch-app-settings <APP_NAME>
```

可能需要登录到 Azure 帐户。

> [!IMPORTANT]  
> 此命令将用 Azure 中函数应用的值覆盖任何现有设置。  
>
> 由于 local.settings.json 文件包含机密，因此永远不会发布它，应将其从源代码管理中排除。  
> 

需要 `AzureWebJobsStorage` 值，即存储帐户连接字符串。 你将使用此连接来验证输出绑定是否按预期方式工作。

