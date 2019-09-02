---
title: Azure Service Fabric 应用程序和群集最佳做法 | Azure
description: 用于管理 Service Fabric 群集和应用程序的最佳做法。
services: service-fabric
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 06/18/2019
ms.date: 08/05/2019
ms.author: v-yeche
ms.openlocfilehash: 6f9bfb691934542682e0ed0ad91eec12b4b8d865
ms.sourcegitcommit: 86163e2669a646be48c8d3f032ecefc1530d3b7f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/02/2019
ms.locfileid: "68753172"
---
# <a name="azure-service-fabric-application-and-cluster-best-practices"></a>Azure Service Fabric 应用程序和群集最佳做法

本文提供了有关管理 Azure Service Fabric 应用程序和群集的最佳做法的链接。 我们强烈建议你实施这些做法，以优化生产环境的可靠性。 使用 [Service Fabric 群集模板之一](https://github.com/Azure-Samples/service-fabric-cluster-templates)开始设计生产解决方案，或更新现有模板以纳入这些做法。

## <a name="security"></a>安全性

* [安全性最佳做法](service-fabric-best-practices-security.md)

## <a name="networking"></a>网络

* [网络最佳做法](service-fabric-best-practices-networking.md)

## <a name="compute-planning-and-scaling"></a>计算规划和缩放

* [计算缩放最佳做法](service-fabric-best-practices-capacity-scaling.md)
* [计算容量规划](/service-fabric/service-fabric-cluster-capacity)

## <a name="infrastructure-as-code"></a>基础结构即代码

* [用于实现基础结构即代码的最佳做法](service-fabric-best-practices-infrastructure-as-code.md)

<!--Not Available on ## Monitoring and diagnostics-->
<!--Not Available on * [Best practices for cluster monitoring and diagnostics](service-fabric-best-practices-monitoring.md)-->

## <a name="application-design"></a>应用程序设计

* [应用程序设计的最佳做法](service-fabric-best-practices-applications.md)

## <a name="checklist"></a>清单

实施前面几节中建议的做法后，请确保已将所有最佳做法集成到生产就绪情况核对清单中：
* [Azure Service Fabric 生产就绪情况核对清单](/service-fabric/service-fabric-production-readiness-checklist)

## <a name="next-steps"></a>后续步骤

* 在运行 Windows Server 的 VM 或计算机上创建群集：[创建适用于 Windows Server 的 Service Fabric 群集](service-fabric-cluster-creation-for-windows-server.md)
* 在运行 Linux 的 VM 或计算机上创建群集：[创建 Linux 群集](service-fabric-cluster-creation-via-portal.md)
* Service Fabric 故障排除：[故障排除指南](https://github.com/Azure/Service-Fabric-Troubleshooting-Guides)

<!--Update_Description: wording update -->
