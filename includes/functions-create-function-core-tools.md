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
ms.openlocfilehash: 6704c2f55911623e5fb508d2ec76b7b595534ab9
ms.sourcegitcommit: bfd0b25b0c51050e51531fedb4fca8c023b1bf5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52673187"
---
## <a name="create-a-function"></a>创建函数

以下命令创建一个名为 `MyHtpTrigger` 的 HTTP 触发的函数。

```bash
func new --name MyHttpTrigger --template "HttpTrigger"
```

执行该命令时，会看到如下所示的输出：

```output
The function "MyHttpTrigger" was created successfully from the "HttpTrigger" template.
```

<!-- ms.date: 11/21/2018 -->