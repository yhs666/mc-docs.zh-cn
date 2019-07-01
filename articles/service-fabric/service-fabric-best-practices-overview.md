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
origin.date: 01/23/2019
ms.date: 03/04/2019
ms.author: v-yeche
ms.openlocfilehash: 4fe9f94b710240c7cd4c6a99fdd1144ed6e0b732
ms.sourcegitcommit: 1ea0f453e7dcaef67f3c52747778c7f3b82e3e38
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67277533"
---
# <a name="azure-service-fabric-application-and-cluster-best-practices"></a>Azure Service Fabric 应用程序和群集最佳做法

为了成功管理 Azure Service Fabric 应用程序和群集，强烈建议你执行一些操作来优化生产环境的可靠性；请执行本文档中定义的操作，并选择我们的 [Azure 示例 Service Fabric 群集模板](https://github.com/Azure-Samples/service-fabric-cluster-templates)之一来开始设计你的生产解决方案或修改现有模板来包括这些做法。

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

## <a name="checklist"></a>清单

完成上面的所有部分后，请确保你已集成了生产就绪情况核对清单中的所有最佳做法：
* [Azure Service Fabric 生产就绪情况核对清单](/service-fabric/service-fabric-production-readiness-checklist)

## <a name="next-steps"></a>后续步骤

* 在运行 Windows Server 的 VM 或计算机上创建群集：[创建适用于 Windows Server 的 Service Fabric 群集](service-fabric-cluster-creation-for-windows-server.md)
* 在运行 Linux 的 VM 或计算机上创建群集：[创建 Linux 群集](service-fabric-cluster-creation-via-portal.md)
* 故障排除：[Service Fabric 故障排除指南](https://github.com/Azure/Service-Fabric-Troubleshooting-Guides)

<!--Update_Description: new articles on service fabric best practices overview -->
<!--ms.date: 03/04/2019-->