---
title: include 文件
description: include 文件
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
origin.date: 09/12/2018
ms.date: 10/19/2018
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: bf4eb95b1fc5fde7a543d32e9c60b2b4940f3a11
ms.sourcegitcommit: 2d33477aeb0f2610c23e01eb38272a060142c85d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2018
ms.locfileid: "49454063"
---
特定函数应用中所有函数的代码均位于根项目文件夹中，其中包含主机配置文件和一个或多个子文件夹。 每个子文件夹包含单独函数的代码，表示形式如下：

```
FunctionApp
 | - host.json
 | - Myfirstfunction
 | | - function.json
 | | - ...  
 | - mysecondfunction
 | | - function.json
 | | - ...  
 | - SharedCode
 | - bin
```

在 2.x 版 Functions 运行时中，函数应用中的所有函数都必须使用相同的语言辅助角色。  

[host.json](../articles/azure-functions/functions-host-json.md) 文件包含一些特定于运行时的配置，位于函数应用的根文件夹中。 `bin` 文件夹包含函数应用所需的包和其他库文件。 查看函数应用项目的语言特定要求：

- [C# 类库 (.csproj)](../articles/azure-functions/functions-dotnet-class-library.md#functions-class-library-project)
- [C# 脚本 (.csx)](../articles/azure-functions/functions-reference-csharp.md#folder-structure)
- [F# 脚本](../articles/azure-functions/functions-reference-fsharp.md#folder-structure)
- [Java](../articles/azure-functions/functions-reference-java.md#folder-structure)
- [JavaScript](../articles/azure-functions/functions-reference-node.md#folder-structure)


<!-- ms.date: 10/19/2018 -->

