---
author: diberry
ms.author: v-junlch
ms.service: cognitive-services
ms.topic: include
origin.date: 01/02/2019
ms.date: 03/01/2019
ms.openlocfilehash: d41541a101732a1c40801d784e96df021d4029eb
ms.sourcegitcommit: ea33f8dbf7f9e6ac90d328dcd8fb796241f23ff7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/01/2019
ms.locfileid: "57204206"
---
`Logging` 设置管理容器的 ASP.NET Core 日志记录支持。 可对容器使用用于 ASP.NET Core 应用程序的相同配置设置和值。 

容器支持以下日志记录提供程序：

|提供程序|目的|
|--|--|
|[Console](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#console-provider)|ASP.NET Core `Console` 日志记录提供程序。 支持此日志记录提供程序的所有 ASP.NET Core 配置设置和默认值。|
|[调试](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#debug-provider)|ASP.NET Core `Debug` 日志记录提供程序。 支持此日志记录提供程序的所有 ASP.NET Core 配置设置和默认值。|
|[Disk](#disk-logging)|JSON 日志记录提供程序。 此日志记录提供程序将日志数据写入输出装入点。|

### <a name="disk-logging"></a>Disk 日志记录

`Disk` 日志记录提供程序支持以下配置设置：  

| Name | 数据类型 | 说明 |
|------|-----------|-------------|
| `Format` | String | 日志文件的输出格式。<br/> **注意：** 此值必须设置为 `json` 才能启用日志记录提供程序。 如果指定了此值，但未同时在实例化容器时指定输出装入点，则会发生错误。 |
| `MaxFileSize` | Integer | 日志文件的最大大小，以 MB 为单位。 如果当前日志文件的大小达到或超过此值，则日志记录提供程序会启动新的日志文件。 如果指定 -1，则日志文件的大小仅受输出装入点的最大文件大小（如果有）的限制。 默认值为 1。 |

有关配置 ASP.NET Core 日志记录支持的详细信息，请参阅[设置文件配置](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1)。

<!-- ms.date: 03/01/2019 -->