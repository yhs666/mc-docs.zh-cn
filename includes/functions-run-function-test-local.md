---
title: include 文件
description: include 文件
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
origin.date: 10/20/2018
ms.date: 09/05/2019
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: ee912876a58691ff96f8be3630bfd9f40430057e
ms.sourcegitcommit: 4f1047b6848ca5dd96266150af74633b2e9c77a3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2019
ms.locfileid: "70805759"
---
## <a name="run-the-function-locally"></a>在本地运行函数

以下命令启动函数应用。 该应用将使用 Azure 中的相同 Azure Functions 运行时运行。 启动命令因项目语言而异。

### <a name="c"></a>C\#

```command
func start --build
```

### <a name="javascript"></a>Javascript

```command
func start
```

### <a name="typescript"></a>TypeScript

```command
npm install
npm start     
```

Functions 主机启动时，会写入类似于以下输出的内容。为了可读性，这些输出已经被截断：

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

