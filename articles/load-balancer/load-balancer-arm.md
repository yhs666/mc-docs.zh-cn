---
title: Azure 资源管理器对负载均衡器的支持
description: 在本文中，将 Azure PowerShell 和模板与 Azure 负载均衡器配合使用
services: load-balancer
documentationcenter: na
author: WenJason
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 11/19/2019
ms.date: 12/09/2019
ms.author: v-jay
ms.openlocfilehash: c9faf2aebb956bedeb2b2a3d7ee7272a6f96f96e
ms.sourcegitcommit: 8c3bae15a8a5bb621300d81adb34ef08532fe739
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2019
ms.locfileid: "74884049"
---
# <a name="azure-resource-manager-support-with-azure-load-balancer"></a>Azure 资源管理器对 Azure 负载均衡器的支持



Azure Resource Manager 是 Azure 中的首选服务管理框架。 Azure 负载均衡器可使用基于 Azure Resource Manager 的 API 和工具进行管理。

## <a name="concepts"></a>概念

借助 Resource Manager，Azure 负载均衡器包含以下子资源：

* 前端 IP 配置 - 单个负载均衡器可包含一个或多个前端 IP 地址（也称为虚拟 IP，即 VIP）。 这些 IP 地址充当流量的入口。
* 后端地址池 - 此池是与负载分布到的虚拟机网络接口卡 (NIC) 关联的 IP 地址的集合。
* 负载均衡规则 - 规则属性将给定的前端 IP 和端口组合映射到一组后端 IP 地址和端口组合。 单个负载均衡器可具有多个负载均衡规则。 每个规则都包含前端 IP 和端口，以及与 VM 关联的后端 IP 和端口。
* 探测：使用探测可以跟踪 VM 实例的运行状况。 如果运行状况探测失败，VM 实例会自动从轮转列表中删除。
* 入站 NAT 规则 - 定义流过前端 IP 并分配到后端 IP 的入站流量的 NAT 规则。

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a>快速入门模板

可以通过 Azure 资源管理器使用声明性模板预配应用程序。 在单个模板中，可以部署多个服务及其依赖项。 在应用程序生命周期的每个阶段，可使用相同模板重复部署应用程序。

模板可能包含以下项的定义：
* **虚拟机**
* **虚拟网络**
* **可用性集**
* **网络接口 (NIC)**
* **存储帐户**
* **负载均衡器**
* **网络安全组**
* **公共 IP** 

使用模板，可创建复杂应用程序所需的一切内容。 模板文件可签入到内容管理系统进行版本控制和协作。

[详细了解模板](../azure-resource-manager/resource-manager-template-walkthrough.md)

[详细了解网络资源](../networking/networking-overview.md)

有关使用 Azure 负载均衡器的快速启动模板，请参阅 [GitHub 存储库](https://github.com/Azure/azure-quickstart-templates)（托管社区生成的模板集）。

模板示例：

* [负载均衡器中的 2 个 VM 和负载均衡规则](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules/)
* [VNET 中包含内部负载均衡器和负载均衡器规则的 2 个 VM](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer/)
* [负载均衡器中的 2 个 VM，在 LB 上配置 NAT 规则](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-natrules/)

## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a>使用 PowerShell 或 CLI 设置 Azure 负载均衡器

Azure 资源管理器 cmdlet、命令行工具和 REST API 入门

* [Azure 网络 Cmdlet](https://docs.microsoft.com/powershell/module/az.network#networking) 可用于创建负载均衡器。
* [如何使用 Azure Resource Manager 创建负载均衡器](load-balancer-get-started-ilb-arm-ps.md)
* [将 Azure CLI 与 Azure 资源管理结合使用](../xplat-cli-azure-resource-manager.md)
* [Load Balancer REST APIs（负载均衡器 REST API）](https://msdn.microsoft.com/library/azure/mt163651.aspx)

## <a name="next-steps"></a>后续步骤

[开始创建面向 Internet 的负载均衡器](load-balancer-get-started-internet-arm-ps.md)，并为特定网络流量行为配置[分发模式](load-balancer-distribution-mode.md)类型。

了解如何管理[负载均衡器的空闲 TCP 超时设置](load-balancer-tcp-idle-timeout.md)。 当应用程序需要为负载均衡器后面的服务器将连接保持活动状态时，这些设置很重要。
