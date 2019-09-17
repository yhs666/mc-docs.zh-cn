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
ms.openlocfilehash: c90b8a3d37a1adaac60473cbf739026d33e2f9f7
ms.sourcegitcommit: 4f1047b6848ca5dd96266150af74633b2e9c77a3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2019
ms.locfileid: "70805753"
---
## <a name="create-a-function"></a>创建函数

以下命令创建一个名为 `MyHttpTrigger` 的 HTTP 触发的函数。

```command
func new --name MyHttpTrigger --template "HttpTrigger"
```

执行该命令时，会看到如下所示的输出：

```output
The function "MyHttpTrigger" was created successfully from the "HttpTrigger" template.
```

