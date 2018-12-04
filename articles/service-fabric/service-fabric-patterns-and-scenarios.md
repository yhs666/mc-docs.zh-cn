---
title: Azure Service Fabric 模式和应用场景 | Azure
description: 了解最佳做法和经验证的可重复使用的模式，以便在 Service Fabric 上设计、开发和操作微服务。
services: service-fabric
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: d5aa75ff-98b9-4573-a2e5-7f5ab288157a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 01/19/2018
ms.date: 10/15/2018
ms.author: v-yeche
ms.openlocfilehash: cd439d0d9c7a03039efefd82b37d04529052819d
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52667241"
---
# <a name="service-fabric-patterns-and-scenarios"></a>Service Fabric 模式和方案
如果你正在考虑使用 Azure Service Fabric 构建大规模的微服务，则可以向设计和构建此平台即服务 (PaaS) 的专家咨询。 从构建合适的体系结构开始，并了解如何优化应用程序的资源。 [Service Fabric Patterns and Practices](https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344) （Service Fabric 模式和实践）课程回答了实际客户最常询问的有关 Service Fabric 应用场景和应用领域的问题。

了解如何使用最佳做法和经验证的可重复使用的模式在 Service Fabric 上设计、开发和操作微服务。 了解 Service Fabric 的基本知识，并深入探讨相关主题，包括群集优化和安全性、迁移旧的应用、大规模的 IoT、托管游戏引擎等等。 了解各种工作负荷的持续交付，甚至获取有关 Linux 支持和容器的详细信息。 

## <a name="introduction"></a>简介
探索最佳实践，并了解为何选择平台即服务 (PaaS)，而不选择基础结构即服务 (IaaS)。 获取以下经过验证的应用程序设计原则的详细信息。

<table><tr><th>视频</th><th>PowerPoint 幻灯片</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=N2KwbbSGD_6405167344">
<img src="./media/service-fabric-patterns-and-scenarios/intro.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344">Service Fabric 简介</a></td></tr>
</table>

## <a name="cluster-planning-and-management"></a>群集规划和管理
在此 Azure Service Fabric 概览中了解容量规划、群集优化和群集安全。

<table><tr><th>视频</th><th>PowerPoint 幻灯片</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=cyDYZcSGD_2805167344">
<img src="./media/service-fabric-patterns-and-scenarios/cluster.png" WIDTH="360" HEIGHT="244">
</a></td><td> <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=E5B3nJSGD_805167344">群集规划和管理</a></td></tr>
</table>

## <a name="hyper-scale-web"></a>超大规模 web
查看有关超大规模 web 的概念，包括可用性和可靠性、超大规模和状态管理。

<table><tr><th>视频</th><th>PowerPoint 幻灯片</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=NgldAdSGD_405167344">
<img src="./media/service-fabric-patterns-and-scenarios/hyperscaleweb.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=CPMLBLSGD_7705167344">超大规模 Web</a></td></tr>
</table>

## <a name="iot"></a>IoT
探索 Azure Service Fabric 环境中的物联网 (IoT)，包括 Azure IoT 管道、多租户和大规模 IoT。

<table><tr><th>视频</th><th>PowerPoint 幻灯片</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=naFUVeSGD_1505167344">
<img src="./media/service-fabric-patterns-and-scenarios/iot.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">IoT</a></td></tr>
</table>

## <a name="gaming"></a>游戏
了解回合制游戏、交互式游戏和托管现有游戏引擎。

<table><tr><th>视频</th><th>PowerPoint 幻灯片</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=6wECzeSGD_3805167344">
<img src="./media/service-fabric-patterns-and-scenarios/gaming.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">游戏</a></td></tr>
</table>

## <a name="continuous-delivery"></a>持续交付
探讨一些概念，包括使用 Azure Pipelines 的持续集成/持续交付、生成/打包/发布工作流、多环境安装和服务包/共享。

<table><tr><th>视频</th><th>PowerPoint 幻灯片</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=78h5ofSGD_305167344">
<img src="./media/service-fabric-patterns-and-scenarios/cd.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=VlENvOSGD_105167344">持续交付</a></td></tr>
</table>

## <a name="migration"></a>迁移
了解如何从云服务迁移，包括迁移旧应用。

<table><tr><th>视频</th><th>PowerPoint 幻灯片</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=hd1cMgSGD_5605167344">
<img src="./media/service-fabric-patterns-and-scenarios/migration.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=GQAq4QSGD_8305167344">迁移</a></td></tr>
</table>

## <a name="containers-and-linux-support"></a>容器和 Linux 支持
获取问题“为什么使用容器？”的答案 了解 Windows 容器、Linux 支持和 Linux 容器业务流程的预览。 此外，了解如何将 .NET Core 应用迁移到 Linux。

<table><tr><th>视频</th><th>PowerPoint 幻灯片</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=V1ERJhSGD_305167344">
<img src="./media/service-fabric-patterns-and-scenarios/containers.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mlYsZRSGD_2105167344">容器和 Linux 支持</a></td></tr>
</table>

## <a name="next-steps"></a>后续步骤
现在，你已了解 Service Fabric 模式和方案，请详细了解如何[创建和管理群集](service-fabric-deploy-anywhere.md)、[将云服务应用迁移到 Service Fabric](service-fabric-cloud-services-migration-worker-role-stateless-service.md)、[设置持续交付](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)以及[部署容器](service-fabric-containers-overview.md)。

<!--Update_Description: update meta properties, wording update -->