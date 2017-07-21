---
title: "Azure 应用服务中的 WebJobs"
description: "了解如何构建 WebJobs，以便运行后台测试、与存储和服务总线之类的服务交互，以及创建计划的任务。"
services: app-service
documentationcenter: 
author: christopheranderson
manager: erikre
editor: mollybos
ms.assetid: 85975432-04c9-4b83-b937-b30c082d52a1
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 12/10/2015
ms.date: 09/26/2016
ms.author: v-dazen
ms.openlocfilehash: eaf76469b0d318efc68d92669a7c2552a373e5b1
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="using-webjobs-in-azure-app-service"></a>在 Azure 应用服务中使用 WebJobs
本文提供了有关如何使用 Azure WebJobs 和 Azure WebJobs SDK 的文档资源的链接。 通过使用 Azure WebJobs，可以轻松地在[应用服务 Web 应用](/app-service-web/app-service-changes-existing-services)上以后台进程的形式运行脚本或程序。 可以上传和运行可执行文件，例如 cmd、bat、exe (.NET)、ps1、sh、php、py、js 和 jar。 这些程序将按计划 (cron) 或者连续地以 Web 作业的形式运行。

借助 WebJobs SDK 可以更方便地使用 Azure 存储。 WebJobs SDK 有一个绑定和触发系统，适用于 Azure 存储 Blob、队列、表以及服务总线队列。

使用 Visual Studio 中集成的工具，可以顺利地创建、部署和管理 Web 作业。 可以从模板创建 Web 作业，还可以发布和管理（运行/停止/监视/调试）这些作业。

Azure 门户中的 Web 作业仪表板提供了强大的管理功能，可让你全面控制 Web 作业的执行，包括调用 Web 作业中的各个函数。 该仪表板还会显示函数运行时和日志记录输出。

[!INCLUDE [app-service-blueprint-webjobs](../../includes/app-service-blueprint-webjobs.md)]