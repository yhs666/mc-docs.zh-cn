---
title: include 文件
description: include 文件
services: app-service
author: ggailey777
ms.service: app-service
ms.topic: include
origin.date: 02/19/2019
ms.date: 11/25/2019
ms.author: v-tawe
ms.custom: include file
ms.openlocfilehash: 32367a2dda1ef04ca6d67db9630e202273c0fbf7
ms.sourcegitcommit: e7dd37e60d0a4a9f458961b6525f99fa0e372c66
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2019
ms.locfileid: "74555919"
---
> [!NOTE]
> Web 应用可在进入非活动状态 20 分钟后超时。 只有向实际 Web 应用发出的请求才会重置计时器。 在 Azure 门户中查看应用的配置或向高级工具站点 (`https://<app_name>.scm.chinacloudsites.cn`) 发出请求不会重置计时器。 如果应用运行连续性或计划的（计时器触发器）WebJobs，可启用 **Always On** 来确保 WebJobs 可靠运行。 此功能仅在基本、标准和高级[定价层](https://www.azure.cn/zh-cn/pricing/details/app-service/)中提供。
