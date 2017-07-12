---
title: "Microsoft Azure 门户概述"
description: "了解如何使用 Azure 门户。"
services: 
documentationCenter: 
authors: davidwrede
manager: dwrede
editor: jimbe
ms.service: na
ms.topic: article
origin.date: 12/16/2015
ms.date: 05/13/2016
ms.author: v-junlch
ms.openlocfilehash: cb57d1bea565ee4cdea2ddfe27b40b042f782517
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
<a id="microsoft-azure-portal-overview" class="xliff"></a>

# Microsoft Azure 门户概述

Microsoft Azure 门户是一个中心位置，可在其中预配和管理 Azure 资源。  本教程将帮助你熟悉此门户，并介绍如何使用以下一些关键功能：
- **综合应用商店**，可在其中浏览数千个来自 Microsoft 和其他供应商的应用，可以购买这些应用和/或对其进行预配。
- **统一且可升级的浏览体验**，可以轻松查找关注的资源并执行各种管理操作。
- **一致的管理页面**（或边栏选项卡），以一致的方式显示设置、操作、计费信息、运行状况监视和使用情况数据等等，使你能够管理 Azure 的各种服务。
- **个性化体验**，可以创建自定义的开始屏幕，其中显示当你登录时想要看见的信息。  还可以自定义包含磁贴的任意管理边栏选项卡。

 ![熟悉 Azure 门户 UI][UIOrientation]

<a id="before-you-get-started" class="xliff"></a>

## 准备工作

必须拥有有效的 Azure 订阅才能完整浏览本教程。  如果没有 Azure 订阅，可立即[注册免费试用版](https://www.azure.cn/pricing/1rmb-trial/)。  获取订阅后，即可通过 [https://manage.windowsazure.cn] 访问门户。

<a id="how-to-create-a-resource" class="xliff"></a>

## 如何创建资源

Azure 应用商店提供数千商品，你可以在一个位置集中创建商品。  假设你要创建一个新的 Windows Server 2012 VM。  “+新建”中心是从应用商店进入特色类别的精选组的入口点。  每个类别都有少量的特色项目，以及指向完整应用商店（显示所有类别和搜索）的链接。 若要新建 Windows Server 2012 VM，请执行以下操作：  

1. Windows Server 2012 是一种特色类别，可以从计算类别中选择它。  
2. 在表单上填写一些基本输入信息。
3. 单击“创建”。此时，系统会立即开始预配你的 VM。

资源已完成创建时，通知中心会发出提醒，并且管理边栏选项卡也会打开（稍后始终可以浏览资源）。

![门户类别][PortalCategories]

<a id="how-to-find-your-resources" class="xliff"></a>

## 如何查找资源

始终可以将经常访问的资源固定到启动板，但也可能需要浏览不经常访问的内容。  可以通过浏览中心（如下所示）访问所有资源。  可以按订阅进行筛选、选择列/调整列大小，并能通过单击各个项导航到管理边栏选项卡。

![浏览中心][BrowseHub]

<a id="how-to-manage-and-delegate-access-to-a-resource" class="xliff"></a>

## 如何管理和委派资源访问权限

在此边栏选项卡中，可以使用远程桌面连接到虚拟机、监视关键性能指标、使用基于角色的访问 (RBAC) 控制对此 VM 的访问、配置 VM，并能执行其他重要的管理任务。  根据角色委派访问权限对大规模管理至关重要。  有关详情，请单击[此处](./active-directory/role-based-access-control-configure.md)。 若要委派资源访问权限，请执行以下操作：

1. 浏览到你的资源。
2. 单击“概要”部分中的“所有设置”。
3. 单击设置列表中的“用户”。
4. 单击命令栏中的“添加”。
5. 选择用户和角色。

![管理资源][ManageResource]

<a id="how-to-customize-a-resource-blade" class="xliff"></a>

## 如何自定义资源边栏选项卡

Azure 将为你的资源预配置边栏选项卡，但这些边栏选项卡上的磁贴由你控制。  可以轻松转到自定义模式来添加、删除、调整或重新排列磁贴。 若要自定义边栏选项卡，请执行以下操作：

1. 浏览到你的资源。
2. 单击 要自定义的边栏选项卡顶部的“...”。
3. 单击“添加部件”。
4. 开始拖放部件。  

![自定义边栏选项卡][CustomizeBlades]

<a id="how-to-get-help" class="xliff"></a>

## 如何获取帮助

如果遇到问题，请随时与我们联系。  门户提供帮助和支持页面，可为你指引正确的方向。  根据[支持计划](http://azure.microsoft.com/support/plans/)，还可以直接在门户中创建支持请求。  创建支持请求之后，可以在门户内管理请求的生命周期。 可以导航到“浏览”->“帮助 + 支持”，访问帮助和支持页面。  

![帮助和支持][HelpSupport]

<a id="summary" class="xliff"></a>

## 摘要

让我们回顾一下本教程的讲解内容：
- 如何注册、获取订阅，并浏览到门户
- 在门户 UI 中导航，以及如何创建和浏览资源
- 如何创建资源和浏览资源
- 结构或管理边栏选项卡，以及如何统一管理不同类型的资源
- 如何自定义门户，以便将关注的信息置于前面或中心位置
- 如何使用基于角色的访问 (RBAC) 控制对资源的访问
- 如何获得帮助和支持

Microsoft Azure 门户大大简化了在云中构建和管理应用程序的工作。  请参阅[管理博客](https://azure.microsoft.com/blog/topics/management/)，了解最新动态，因为我们一直坚持[聆听反馈](https://feedback.azure.com/forums/223579-azure-preview-portal/)并做出改进。  [ScottGu 的博客](http://weblogs.asp.net/scottgu)是查找所有 Azure 更新的另一个极佳位置。

[UIOrientation]: ./media/azure-portal-how-to-use/azure_portal_1.png
[PortalCategories]: ./media/azure-portal-how-to-use/azure_portal_2.png
[BrowseHub]: ./media/azure-portal-how-to-use/azure_portal_3.png
[ManageResource]: ./media/azure-portal-how-to-use/azure_portal_4.png
[CustomizeBlades]: ./media/azure-portal-how-to-use/azure_portal_5.png
[HelpSupport]: ./media/azure-portal-how-to-use/azure_portal_6.png