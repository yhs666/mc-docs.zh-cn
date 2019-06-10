---
ms.openlocfilehash: b1e1885bc340b8607c456c26d486ed4f79685487
ms.sourcegitcommit: 9e839c50ac69907e54ddc7ea13ae673d294da77a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2019
ms.locfileid: "66491465"
---
若要引用 Azure Functions 2.x 默认绑定，请打开 *host.json* 文件并更新内容以匹配以下代码。

```json
{
    "version": "2.0",
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    }
}
```

