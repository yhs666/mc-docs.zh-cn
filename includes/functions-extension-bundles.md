---
author: ggailey777
ms.service: azure-functions
ms.topic: include
origin.date: 05/27/2019
ms.date: 09/05/2019
ms.author: v-junlch
ms.openlocfilehash: 79c72c85a463147613d70e89d6fdb244c7be2504
ms.sourcegitcommit: 4f1047b6848ca5dd96266150af74633b2e9c77a3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2019
ms.locfileid: "70805743"
---
安装绑定扩展的最简单方法是启用[扩展捆绑包](../articles/azure-functions/functions-bindings-register.md#extension-bundles)。 启用捆绑包时，会自动安装一组预定义的扩展包。

若要启用扩展捆绑包，请打开 host.json 文件并更新其内容以匹配以下代码：

```json
{
    "version": "2.0",
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    }
}
```

