---
title: 查看 Azure Service Fabric 中的容器日志 | Azure
description: 介绍在使用 Service Fabric Explorer 运行 Service Fabric 容器服务时，如何查看相应的容器日志。
services: service-fabric
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 04/09/2018
ms.date: 04/23/2018
ms.author: v-yeche
ms.openlocfilehash: f93257f73952538976a6aa007fe303f46b1497a3
ms.sourcegitcommit: 6f08b9a457d8e23cf3141b7b80423df6347b6a88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2018
---
# <a name="view-logs-for-a-service-fabric-container-service"></a>查看 Service Fabric 容器服务的日志
Azure Service Fabric 是一种容器业务流程协调程序，支持 [Linux 和 Windows 容器](service-fabric-containers-overview.md)。  本文介绍如何查看正在运行的容器服务的容器日志，以便诊断和排查问题。

## <a name="access-container-logs"></a>访问容器日志
可以通过 [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) 来访问容器日志。  在 Web 浏览器中导航到 [http://mycluster.region.cloudapp.chinacloudapi.cn:19080/Explorer](http://mycluster.region.cloudapp.chinacloudapi.cn:19080/Explorer)，从群集的管理终结点打开 Service Fabric Explorer。  

容器日志位于容器服务实例运行时所在的群集节点。 例如，可以获取 [Linux Voting 示例应用程序](service-fabric-quickstart-containers-linux.md)的 Web 前端容器的日志。 在树状视图中，展开“群集”>“应用程序”>“VotingType”>“fabric:/Voting/azurevotefront”。  然后展开分区（在此示例中为 d1aa737e-f22a-e347-be16-eec90be24bc1），此时会看到容器在群集节点 *_lnxvm_0* 上运行。

在树状视图中找到 *_lnxvm_0* 节点上的代码包，方法是展开“节点”>“_lnxvm_0”>“fabric:/Voting”>“azurevotfrontPkg”>“代码包”>“代码”。  然后选择“容器日志”选项，以显示容器日志。

![Service Fabric 平台][Image1]

## <a name="next-steps"></a>后续步骤
<!-- Not Available on - Work through the [Create a Linux container application tutorial](service-fabric-tutorial-create-container-images.md)-->
- 详细了解 [Service Fabric 和容器](service-fabric-containers-overview.md)

[Image1]: media/service-fabric-containers-view-logs/view-container-logs-sfx.png
<!-- Update_Description: new articles on service fabric containers view logs  -->
<!--ms.date: 04/23/2018-->