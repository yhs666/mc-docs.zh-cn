---
title: include 文件
description: include 文件
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
origin.date: 09/27/2018
ms.date: 10/19/2018
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 1f1fbf5256dcb053d048b947abde2b55b6433ea7
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52647635"
---
## <a name="deploy-the-function-app-project-to-azure"></a>将函数应用项目部署到 Azure

在 Azure 中创建函数应用后，可以使用 [`func azure functionapp publish`](../articles/azure-functions/functions-run-local.md#project-file-deployment) 命令将项目代码部署到 Azure。

```bash
func azure functionapp publish <FunctionAppName>
```

你将看到如下输出，为了可读性，这些输出已经被截断。

```output
Getting site publishing info...

...

Preparing archive...
Uploading content...
Upload completed successfully.
Deployment completed successfully.
Syncing triggers...
```

现在可以在 Azure 中测试函数。

<!-- ms.date: 10/19/2018 -->