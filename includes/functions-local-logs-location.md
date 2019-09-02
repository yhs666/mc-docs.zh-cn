---
title: include 文件
description: include 文件
services: functions
author: ggailey777
manager: cfowler
ms.service: functions
ms.topic: include
origin.date: 05/01/2018
ms.date: 05/30/2018
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 514cef1fa426162b047e758fef77882ffde2917f
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52652661"
---
当 Functions 主机在本地运行时，它会将日志写入以下路径：

```
<DefaultTempDirectory>\LogFiles\Application\Functions
```

在 Windows 上，`<DefaultTempDirectory>` 是 TMP、TEMP、USERPROFILE 环境变量或 Windows 目录的第一个找到的值。
在 MacOS 或 Linux 上，`<DefaultTempDirectory>` 是 TMPDIR 环境变量。

> [!NOTE]
> Functions 主机在启动时覆盖目录中的现有文件结构。

<!-- ms.date: 05/30/2018 -->