---
title: include 文件
description: include 文件
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
origin.date: 10/20/2018
ms.date: 12/26/2018
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 9318f132add099c2fd6ad7d17883740b4bdedf8f
ms.sourcegitcommit: d15400cf780fd494d491b2fe1c56e312d3a95969
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/28/2018
ms.locfileid: "53806641"
---
## <a name="create-a-function"></a>创建函数

以下命令创建一个名为 `MyHttpTrigger` 的 HTTP 触发的函数。

```bash
func new --name MyHttpTrigger --template "HttpTrigger"
```

执行该命令时，会看到如下所示的输出：

```output
The function "MyHttpTrigger" was created successfully from the "HttpTrigger" template.
```

<!-- ms.date: 12/26/2018 -->