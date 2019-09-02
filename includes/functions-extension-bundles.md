---
author: ggailey777
ms.service: azure-functions
ms.topic: include
origin.date: 05/27/2019
ms.date: 07/17/2019
ms.author: v-junlch
ms.openlocfilehash: 58d0b71fcf23d35f1f4b51fd8a50274fc2eeacc2
ms.sourcegitcommit: c61b10764d533c32d56bcfcb4286ed0fb2bdbfea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/19/2019
ms.locfileid: "68331885"
---
安装绑定扩展的最简单方法是启用[扩展捆绑包](../articles/azure-functions/functions-bindings-register.md#extension-bundles)。 启用捆绑包后，会自动安装一组预定义的扩展包。

若要启用扩展捆绑包，请打开 *host.json* 文件并更新其内容以匹配以下代码：

```json
{
    "version": "2.0",
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    }
}
```

