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
ms.openlocfilehash: 5826b685610824a5d12a325ed0539de3ff42387a
ms.sourcegitcommit: e7dd37e60d0a4a9f458961b6525f99fa0e372c66
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2019
ms.locfileid: "74555928"
---
1. **在“解决方案资源管理器”** 中，右键单击该项目并选择“发布”  。

1. 在“发布”对话框中，依次选择“Microsoft Azure 应用服务”和“新建”，然后选择“发布”。    

   ![选取发布目标](./media/webjobs-publish-netcore/pick-publish-target.png)

1. 在“创建应用服务”  对话框中，使用图片下方的表中指定的托管设置：

    ![“创建应用服务”对话框](./media/webjobs-publish-netcore/app-service-dialog.png)

    | 设置      | 建议的值  | 说明                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **应用名称** | 全局唯一名称 | 用于唯一标识新 Function App 的名称。 |
    | **订阅** | 选择订阅 | 要使用的 Azure 订阅。 |
    |  [资源组](../articles/azure-resource-manager/resource-group-overview.md) | MyResourceGroup |  要在其中创建 Function App 的资源组的名称。 选择“新建”  创建新的资源组。|
    | **[托管计划](../articles/app-service/overview-hosting-plans.md)** | 应用服务计划 | [应用服务计划](../articles/app-service/overview-hosting-plans.md)指定托管应用的 Web 服务器场的位置、大小和功能。 可以在托管多个应用时节省资金，方法是将多个 Web 应用配置为共用单个应用服务计划。 应用服务计划定义区域、实例大小、规模计数和 SKU（免费、共享、基本、标准或高级）。 然后，选择“新建”  以创建一个新的应用服务计划。 |

1. 单击“创建”  以使用这些设置在 Azure 中创建 Web 作业和相关资源，并部署项目代码。