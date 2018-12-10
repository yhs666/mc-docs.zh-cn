---
title: include 文件
description: include 文件
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
origin.date: 09/21/2018
ms.date: 10/19/2018
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: aed9c1b582dbc95c5591fbcd58ff822a98ed76b9
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52645155"
---
在本地开发函数时，可以使用 Azure Functions Core Tools 从终端或从命令提示符安装所需的扩展。

更新 function.json 文件以包含函数所需的所有绑定后，请在项目文件夹中运行以下命令。

```bash
func extensions install
```

该命令读取 function.json 文件以了解所需的程序包，安装这些包并重新生成扩展项目。 它在当前版本中添加任何新绑定，但不更新现有绑定。 使用 `--force` 选项可在安装新版本时将现有绑定更新为最新版本。

如要安装特定版本的包或要在编辑 function.json 文件之前安装包，请对包名称使用 `func extensions install` 命令，如下例所示：

```bash
func extensions install --package Microsoft.Azure.WebJobs.ServiceBus --version <target_version>
```

将 `<target_version>` 替换为特定包版本，例如 `3.0.0-beta5`。 在 [NuGet.org](https://nuget.org) 上的单个包页上列出了有效版本。

<!-- ms.date: 10/19/2018 -->