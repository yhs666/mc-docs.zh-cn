---
title: include 文件
description: include 文件
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
origin.date: 10/20/2018
ms.date: 11/21/2018
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 9394fa5529d67cd5f1b74961e4a69a66a294e57a
ms.sourcegitcommit: bfd0b25b0c51050e51531fedb4fca8c023b1bf5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52673189"
---
## <a name="run-the-function-locally"></a>在本地运行函数

以下命令启动函数应用。 该应用将使用 Azure 中的相同 Azure Functions 运行时运行。

```bash
func host start --build
```

编译 C# 项目需要 `--build` 选项。 不需对 JavaScript 项目使用此选项。

当 Functions 宿主启动时，会写入以下输出所示的内容（为方便阅读，内容已截断）：

```output

                  %%%%%%
                 %%%%%%
            @   %%%%%%    @
          @@   %%%%%%      @@
       @@@    %%%%%%%%%%%    @@@
     @@      %%%%%%%%%%        @@
       @@         %%%%       @@
         @@      %%%       @@
           @@    %%      @@
                %%
                %

...

Content root path: C:\functions\MyFunctionProj
Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

...

Http Functions:

        HttpTrigger: http://localhost:7071/api/MyHttpTrigger

[8/27/2018 10:38:27 PM] Host started (29486ms)
[8/27/2018 10:38:27 PM] Job host started
```

从运行时输出中复制 `HttpTrigger` 函数的 URL，并将其粘贴到浏览器的地址栏中。 将查询字符串 `?name=<yourname>` 追加到此 URL 并执行请求。 下面演示本地函数在浏览器中返回的对 GET 请求的响应：

![在浏览器本地进行测试](./media/functions-run-function-test-local/functions-test-local-browser.png)

在本地运行函数后，可在 Azure 中创建函数应用和其他所需资源。

<!-- ms.date: 11/21/2018 -->