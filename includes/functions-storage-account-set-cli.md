---
author: ggailey777
ms.service: azure-functions
ms.topic: include
origin.date: 09/26/2019
ms.date: 10/28/2019
ms.author: v-junlch
ms.openlocfilehash: 57fdd559ae569f677a053c93a5d7b9cb93e62939
ms.sourcegitcommit: 7d2ea8a08ee329913015bc5d2f375fc2620578ba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034502"
---
### <a name="set-the-storage-account-connection"></a>设置存储帐户连接

打开 local.settings.json 文件并复制 `AzureWebJobsStorage` 的值（即存储帐户连接字符串）。 使用以下 Bash 命令将 `AZURE_STORAGE_CONNECTION_STRING` 环境变量设置为该连接字符串：

```azurecli
AZURE_STORAGE_CONNECTION_STRING="<STORAGE_CONNECTION_STRING>"
```

在 `AZURE_STORAGE_CONNECTION_STRING` 环境变量中设置连接字符串时，无需每次都要提供身份验证，即可访问你的存储帐户。

