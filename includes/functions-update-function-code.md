---
title: include 文件
description: include 文件
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
origin.date: 03/12/2019
ms.date: 03/25/2019
ms.author: v-junlch
ms.custom: include file, fasttrack-edit
ms.openlocfilehash: 8dd33d8b210c522f3852727bd0cd43fa2277095b
ms.sourcegitcommit: 07a24e9a846705df3b98fc8ff193ec7d9ec913dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2019
ms.locfileid: "58431560"
---
## <a name="update-the-function"></a>更新函数

默认情况下，模板会创建一个在发出请求时需要函数密钥的函数。 若要在 Azure 中更轻松地测试该函数，需将该函数更新为允许匿名访问。 进行此更改的方式取决于函数项目语言。

### <a name="c"></a>C\#

打开 MyHttpTrigger.cs 代码文件（即新函数），将函数定义中的 **AuthorizationLevel** 特性更新为值 `Anonymous`，然后保存更改。

```csharp
[FunctionName("MyHttpTrigger")]
public static async Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)] HttpRequest req,
    ILogger log)
```

### <a name="javascript"></a>Javascript

在文本编辑器中打开新函数的 function.json 文件，将 **bindings.httpTrigger** 中的 **authLevel** 属性更新为 `anonymous`，然后保存更改。

```json
  "bindings": [
    {
      "authLevel": "anonymous",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ]
```

现在，无需提供函数密钥，即可在 Azure 中调用该函数。 在本地运行时永远不需要函数密钥。

